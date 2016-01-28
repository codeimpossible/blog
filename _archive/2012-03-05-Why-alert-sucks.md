---
layout: post
status: publish
title: 'Why alert() sucks'
slug: "Why-alert-sucks"
---

## Alerts are a terrible thing and no website should use them.


![][1] 


Alerts **suck**. In every web project that I work on, the code below is one of the first things I add. It will override the alert function an instead pass the argument supplied to the console.


    // the console && console.log check is to make sure 
    // this code doesn't fail in IE which does not create 
    // the console obj unless the user has dev tools open.
    window.alert = function(m) {
      if(console && console.log) {
        console.log(m);
      }
    };


Why is the code above necessary? Why should you stop using alerts in your production javascript code? Let's take a look.


**Alerts block the javascript execution.** When you call alert the entire javascript thread blocks until the alert dialog is closed. Sounds pretty useful right?


**Alerts block the browser.** Alert dialogs are displayed [modally][2]  which means that the parent thread (the browser) blocks until you close them. 


**They look terrible.** This is pretty self explanatory. I&#39;ve never had an alert fit in with a websites ux. It&#39;s actually pretty jarring when you see one.


**Alerts only display strings.** Console.log can display all sorts of data, and is included in most major browsers (and node.js) which is why I chose to use console.log as a replacement for alert in the snippet above.


**Alerts tend to lead to bad behaviors.** I&#39;ve been working in web development for about 9 years now and in every codebase I&#39;ve worked on there is, somewhere, an alert with profanity or "what the hell", or "WTF", or some kind of rediculous message that a dev left in there to signal when they got to some unexpected block of code.


To this day, some developers still prefer to use this method but with all of the javascript unit test frameworks and in-browser debuggers out there this just doesn't make sense. 


  [1]: http://dl.dropbox.com/u/6291954/15086571.jpg
  [2]: http://en.wikipedia.org/wiki/Modal_window
