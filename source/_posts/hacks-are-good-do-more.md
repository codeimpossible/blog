---
title: "Hacks are good! Do moar!"
date: 2016-01-30 01:31:55
categories:
- development
tags:
- best-practices
---
In January I was lucky enough to participate in a project review session for [General Assembly](https://generalassemb.ly/) a programmer bootcamp here in Austin, TX. I was one of three professional software devs brought in to provide feedback on the students' projects. The feedback can be on anything, from how they approached the UX, their data models, how they used git... anything.

During each presentation each team had a set of topics that they had to talk about, one of them being "rose, bud, thorn" or "what was great about this project/experience", "what will you take on to your next project(s)", and "what sucked".

One team of students began a demo of their search filters for a recipe site they had built. It had a great amazon-esque checkbox filter system where you could pick recipe ingredients to filter the list of recipes on the site. It worked really well.

For their "what sucked" part, one of the team members brought up the code for the checkbox filtering and showed how it worked. There was a collection of around twenty `if` statements that controlled what happened when one of the recipe filters was chosen.

Basically it looked something like this, just 20x as much:

```javascript
$('checkbox1').on('change', function() {
  if ($(this).is(':checked')) {
    $('form #hidden1').val($(this).val());
  } else {
    $('form #hidden1').val('');
  }
});
```

So for each checkbox there was a matching hidden field in a form so the selected search filters could be sent to the server and the search could be carried out. A truly righteous hack, if I may say so.

When the rest of the reviewers and I saw this we kinda chuckled a bit - which to be honest, probably wasn't the best response - but I explained that we were all chuckling because hacks are a common thing in software development and we'd all done hacks like that (or in worse) to make something work to meet a deadline. It's a fact of life. And it's a friggin' important one too.

We should create hacks more often. Don't kill yourself for 2-3 hours to try to find the best/most correct way to do something immediately. If you meet resistance, do the simplest thing that could possibly work - make a hack - and move on. Come back to it - or even better get someone to pair or code review what you've done - and refactor it when you know the better way to do it. Bad code is... well, yeah, bad... but it's also super important since it helps us grow and become better.

You won't forget your hacks easily. They'll stay with you for a while; they will keep you up at night. In fact, one night, you'll probably refactor them into as few lines of code as possible.

And one day you'll come back to those hacks, years from now and chuckle too.
