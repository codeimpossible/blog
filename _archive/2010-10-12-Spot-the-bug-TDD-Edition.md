---
layout: post
status: publish
title: 'Spot the bug: TDD Edition'
slug: "Spot-the-bug-TDD-Edition"
---
## Thanks to TDD I was able to prevent an obscure bug in a third party library from affecting my code before it got to the customer. It felt really awesome to catch this one before it happened in the wild.

For the past two years I've been doing Asp.net MVC development off and on and practicing TDD off and on as well. For my current project at work I'm doing TDD "Full Bore" and I'm absolutely loving it. It's saved my bacon numerous times and I can't imagine doing a long-term, complex project without it.

Today I have one example of how TDD has saved my arse: I'm beginning work on adding a controller for Projects in our db and the Index action needs return a list of projects in the system. Following TDD I've got to write the test first before I get to write any system code.

{% codeblock lang:csharp %}
[Test]
public void Index_HttpGet_ModelIsListOfProjects ()
{
    var controller = new ProjectController();

    var result = controller.Index() as ViewResult;

    // uses WithViewData extension method from
    // MvcContrib TestHelpers
    result.WithViewData<List<Project>>();
}

{% endcodeblock %}

Okay, now, let's create that controller so I can run my test and see it fail.

{% codeblock lang:csharp %}
public class ProjectController : Controller
{
    public ActionResult Index ()
    {
        return View();
    }
}
{% endcodeblock %}


Great! Now to run my test aaaaaaaand. Whoa! What? The test is **passing**!?!? W.T.F?!?

... after about 30 seconds of staring at my computer screen in awe I realize what must be happening. I load up the source for the `WithViewData` extension method and spot the bug.

*Note: I skipped a step here, WithViewData() calls AssertViewDataModelType() so I included that method here instead*

{% codeblock lang:csharp %}
private static TViewData AssertViewDataModelType<TViewData>(ViewResultBase actionResult)
{
    var actualViewData = actionResult.ViewData.Model;
    var expectedType = typeof(TViewData);

    if ( actualViewData == null && expectedType.IsValueType)
    {
        throw new ActionResultAssertionException(
            string.Format("Expected view data of type '{0}', actual was NULL",
            expectedType.Name));
    }

    if (actualViewData == null)
    {
        return (TViewData)actualViewData;
    }

    if (!typeof(TViewData).IsAssignableFrom(actualViewData.GetType()))
    {
        throw new ActionResultAssertionException(
            string.Format("Expected view data of type '{0}', actual was '{1}'",
            typeof(TViewData).Name, actualViewData.GetType().Name));
    }

    return (TViewData)actualViewData;
}
{% endcodeblock %}

Can you tell why the test is passing?
