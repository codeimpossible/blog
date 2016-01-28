---
layout: post
status: publish
title: 'An example of using Dynamic objects in your Razor views'
slug: "An-example-of-using-Dynamic-objects-in-your-Razor-views"
---

## @JohnBubriski posted a blog post that got me thinking of how the DynamicObject class, and a little elbow grease, can add some elegance to your MVC views.


My good friend [@JohnBubriski][1]  put up [a great post on his blog][2]  where he talks about accessing url route params in your MVC views.


John made a really good solution by adding an extension method to the HtmlHelper class which would return the `{id}` route param. This solution is simple, any developer can understand it at a glance, but I thought I would show how you might use DynamicObject and the features of .Net 4.0 to solve this problem.
    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using System.Dynamic;
    
    namespace System.Web.Mvc {
    
        public class DynamicUrlAccessor : DynamicObject {
            private ViewContext _context;
            
            public DynamicUrlAccessor(ViewContext context) {
                _context = context;
            }
            
            public override bool TryInvokeMember(
                    InvokeMemberBinder binder, 
                    object[] args, 
                    out object result) {
    
                if (_context.RouteData.Values.ContainsKey(binder.Name)) {
                    result = _context.RouteData.Values[binder.Name].ToString();
                    return true;
                }
    
                // the dirty way too
                var querystring = HttpContext.Current.Request.QueryString;
                if (querystring.AllKeys.Contains(binder.Name)) {
                    result = querystring[binder.Name].ToString();
                    return true;
                }
                
                return base.TryInvokeMember(binder, args, out result);
            }
        }
    
        public static class ContextExtensions {
    
            public static dynamic RouteInfo(this System.Web.Mvc.HtmlHelper helper) {
                var accessor = new DynamicUrlAccessor(helper.ViewContext);
                return accessor;
            }
        }
    }
    



Now you can use this code in your view like so:


    
    &lt;h2&gt;About&lt;/h2&gt;
    &lt;p&gt;
         Hello @Html.RouteInfo().name()! You are in the @Html.RouteInfo().action() action, 
    which is located in the @Html.RouteInfo().controller() controller.
    &lt;/p&gt;
    



![][3] 


When the `RouteInfo()` method is called our DynamicUrlAccessor object is retuned as a `dynamic`. This in turn will cause our overridden method `TryInvokeMember` to be called.

The binder object will contain the name of the method that was invoked, any arguments passed will be in the args[] collection. All we need to do at this point is check whether the ViewContext or the QueryString contains the requested parameter and set its value into the result variable.


So hopefully this shows you how a little work with DynamicObject can go a long way towards improving code readability and creating elegant solutions.


  [1]: https://twitter.com/#!/JohnBubriski
  [2]: http://www.johnnycode.com/blog/2011/11/09/asp-net-mvc-extension-method-for-id-in-route
  [3]: http://content.screencast.com/users/codeimpossible/folders/Jing/media/456e1966-0970-4470-93a9-05789d98c8db/2011-11-09_2255.png
