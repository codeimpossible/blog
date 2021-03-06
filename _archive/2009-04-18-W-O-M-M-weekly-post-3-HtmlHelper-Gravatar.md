---
layout: post
status: publish
title: 'W.O.M.M. weekly post #3 - HtmlHelper.Gravatar'
slug: "W-O-M-M-weekly-post-3-HtmlHelper-Gravatar"
---
![works-on-my-machine-starburst][1] Long story short: I hate re-inventing the wheel. If there is a free service that does something I need I will try my hardest to get that service into whatever I am working on. I'm currently working on an Asp .Net MVC project that needs Avatars (you know, those funny little pictures next to peoples names on Twitter). Enter [Gravatar][2] .

Gravatar is an awesome service for anyone looking to add avatars to their apps. It's free, incredibly simple to implement, and removes a lot of the hassle around getting avatar support into your web/windows app.

Adding Gravatar support to an application is pretty simple. Can you get a users email address? Can you MD5 said email address? Can you make an HTTP GET? BANG. You sir, or madam, can have Gravatars.

This week for the W.O.M.M. code sample I'd like to show how I integrated gravatar support into an Asp .Net MVC application.

How Gravatar Works In A Nutshell
Gravatar is a free service where you sign up and link images to one or more email addresses that you provide.

Once you link an image to an email address, any application that supports getting an image over the internet can show your Gravatar by making a request to a special URL. This URL is generated by combining an MD5 hash of your email address with some other parameters and the end result is a link to your Gravatar image.

IE: the link to my Gravatar on the right of this page is:

`http://www.gravatar.com/avatar/15559d868ec27b8583f42116a6b96c14?s=140`

So `15559d868ec27b8583f42116a6b96c14` is the hash of my email address - don't worry it's a one-way hash. The "s" parameter is the size of the image that I want, in this case 140 pixels.

That is pretty much it as far as how the system works, but if you want to read more, check out [Gravatar's implementation documentation][3] .

The Goal
What I wanted was an HtmlHelper extension method that I could use in my view pages to create an IMG tag with the correct Gravatar URL. After [looking at the documentation on Gravatars "How the URL is constructed" page][4] , I decided my helper extension should support the following:

Avatar Size (the "s" parameter)
When making a Gravatar URL you can specify a specific size for the Gravatar image. The size can be anything from 1 to 512 pixels, but the default is 80.

Default Avatars (the "d" parameter)
If the email address you are using doesn't have any Gravatars setup, Gravatar will generate one for you by default. You can choose from 3 predefined Gravatar types or you can include a URL to a custom avatar of your own. The predefined Gravatar types are Identicon, Wavatar, and Monsterid.

Rating (the "r" parameter)
This wasn't a requirement for what I was working on, but you can designate the maximum "rating" of the avatars that Gravatar will generate. The accepted values are "g", "pg", "r", and "x" and they are inclusive, so specifying "r" will allow "g" and "pg" rated Gravatars to be generated. Gravatars that are rated "x" will be returned as one of the predefined avatars above. The default rating is "g".

The Code
Okay, so now I know what I need to support. Now it's just a matter of getting the code to do this. Let's take a look at the class file I used to get this done.

    
    namespace System.Web.Mvc
    {
        using System;
        using System.Web.Routing;
        using System.Web.Security;
    
        public enum GravatarDefaultTypes
        {
            Identicon,
            Wavatar,
            Monsterid,
            Custom
        }
    
        public static class GravatarExtension
        {
            public static string Gravatar(
                this HtmlHelper hh,
                string emailAddress,
                int size,
                GravatarDefaultTypes defaultType,
                string customImageUrl,
                RouteValueDictionary htmlAttributes)
            {
                var tagBuilder = new TagBuilder("img");
                string url = "http://www.gravatar.com/avatar/{0}?d={1}&s={2}";
    
                // thanks to jon galloway for this one-liner!
                // http://www.eggheadcafe.com/aspnet/how-to/141740/adding-gravatars-to-your.aspx
                string hash = FormsAuthentication.HashPasswordForStoringInConfigFile(emailAddress, "MD5");
                string defImg = defaultType.ToString().ToLower();
    
                if (defaultType == GravatarDefaultTypes.Custom)
                {
                    defImg = System.Web.HttpUtility.UrlEncode(customImageUrl);
                }
    
                url = String.Format(
                    url,
                    hash.ToLower(),
                    defImg,
                    size.ToString());
    
                tagBuilder.MergeAttributes(htmlAttributes);
                tagBuilder.MergeAttribute("src", url);
    
                return tagBuilder.ToString(TagRenderMode.Normal);
            }
        }
    }
    


So you can see I'm not storing the hash of the email address, instead I am going to pass in the unaltered string. I didn't want to have another piece of data to update when the user changed their email address so the Gravatar() method takes an email address and encodes it using a call to FormsAuthentication.HashPasswordForStoringInConfigFile(), which is awesome ( Thanks Jon, you rock!).

Also, I'm not sure if this is a no-no or what, but I did put the extension class under the System.Web.Mvc namespace. This was mainly a convenience (read: laziness) thing and can be easily changed.

Alright so we have some code now, let's take a look at how it can be used in our views.

    
    &lt;%= Html.Gravatar(
        Model.Email, // the email address
        50,          // size, in pixels of the avatar
        GravatarDefaultTypes.Identicon,
        null,
        new RouteValueDictionary(new {
        style = "vertical-align: middle;"
        })
    )%&gt; 
     &lt;%= Model.UserName %&gt; 


Let's see how that looks.

![user-avatar][5] 

Booyah, avatar support in 55 lines of code. As always, if I screwed up or there is a better way to do this, please let me know.

  [1]: http://wpup.codeimpossible.com/2009/06/works-on-my-machine-starburst.jpg
  [2]: http://www.gravatar.com
  [3]: http://en.gravatar.com/site/implement
  [4]: http://en.gravatar.com/site/implement/url
  [5]: http://wpup.codeimpossible.com/2009/04/user-avatar.png
