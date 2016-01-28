---
layout: post
status: publish
title: 'Solving "$(document).ready is not a function" and other problems'
slug: "solving-document-ready-is-not-a-function-and-other-problems"
date: 2010/01/13 00:00:00
---
### Ever been working on a customer's site, writing some really awesome jQuery, you deploy it, and everything is awesome. And then you get an email one day...

Has this ever happened to you: you've been working on a customer's site, writing some really awesome jQuery flashy, fadey, scrolly, interactivey thing, you deploy it, and everything is awesome. The customer rejoices and the customer's customers rejoice. Rejoicing is had by everyone. And then you get an email one day:

> "Everything is broken. We've kidnapped your dog. Fix our site or you'll never see Spartacus again."

And before you have time to wonder why you ever named your dog "Spartacus" to begin with (i mean **come. on.**), you're off in debug hell.

You load the site and see all sorts of weird errors: `"$().ready is not a function"` `"$(document) doesn't support this property or method"` Or my personal favorite: `"null is null or not an object"`

You open up FireFox, activate FireBug, load the console, and type `alert($)`, press run, and instead of seeing the expected jQuery function:

```javascript
    function (E, F) {
        return new (o.fn.init)(E, F);
    }
```

You instead get:

```javascript
    function $(element) {
        if (arguments.length > 1) {
            for (var i = 0, elements = [], length = arguments.length; i < length; i++) {
                elements.push($(arguments[i]));
            }
            return elements;
        }
        if (Object.isString(element)) {
            element = document.getElementById(element);
        }
        return Element.extend(element);
    }
```

Or even:

```javascript
    function $(id) {
        return document.getElementById(id);
    }
```

**DOH!** Looks like another javascript library has been loaded and has overwritten the `$()` shortcut for jQuery. Woe is I. Why can't we all just get along?!? Well, we can't stop people from including their favorite javascript libraries, but what we can do is prevent our code from suffering as a result. We'll need a nice, big beefy, bodyguard to make sure our code isn't messed with while it's out clubbing with Prototype, Scriptaculous or even MooTools (who invited *him*??!?). Here's what our bodyguard function will look like

```javascript
    ( function($) {

    } ) ( jQuery );
```

So what this does is call our anonymous function and pass the `jQuery` object. This will scope `$` our little function so we won't step on anyone else's toes (and they won't bump into us while we're on the dance floor and spill our drink everywhere). Okay, I think I&#39;ve taken the clubbing metaphor far enough.

Basically this will allow our code to run and use the `$` shortcut for JQuery as if it were loaded without any of these other libraries on the page. Here is what the completed code would look like:

```html
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.1/jquery.min.js" type="text/javascript">
    </script>

    <script src="http://ajax.googleapis.com/ajax/libs/prototype/1.6.1.0/prototype.js" type="text/javascript">
    </script>
    <script src="http://ajax.googleapis.com/ajax/libs/scriptaculous/1.8.3/scriptaculous.js" type="text/javascript">
    </script>

    <script type="text/javascript">
    ( function($) {
        // we can now rely on $ within the safety of our "bodyguard" function
        $(document).ready( function() { alert("nyah nyah! I'm able to use '$'!!!!");  } );
    } ) ( jQuery );

    //this will fail
    $(document).ready( function() { alert('fail?'); } );
    </script>
```

I love using this simple self-calling anonymous function style when working with jQuery because it saves me from typing `jQuery()`, which really does look a lot more ugly than using the `$()` shortcut. It also protects my code from any scoping issues and lets the code function normally when [jQuery is put into no conflict mode](http://docs.jquery.com/Core/jQuery.noConflict).

My opinion, if you're doing work in jQuery on sites that you don't control 100%, you should be using this method to protect your code and your clients.

**Updated: changed link for jquery to use 1.4.1 at the google CDN (tsk, tsk, tsk I was using the googlecode.com link)**
