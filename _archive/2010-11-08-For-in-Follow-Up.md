---
layout: post
status: publish
title: 'For/in Follow Up'
slug: "For-in-Follow-Up"
---

## JavaScript has a lot of good parts and bad parts but the for/in loop seems to walk the grey area in between. It's a fantastic feature of JavaScript when wielded appropriately, but a potentially horrible bug when used incorrectly.


A few weeks back [I wrote a post about a bug that can occur when you improperly use a for/in][1]  style loop in javascript.


This is particular quirk is one that I find myself explaining to a lot of devs when they start using JavaScript, especially if they are coming from C# (with it's foreach loop having a similar syntax).


**"I do not think that means what you think it means."**
The for/in loop in JavaScript is powerful but it definitely doesn&#39;t do what you probably think it does. To understand the for/in loop we&#39;ll have to dive deeper into objects and arrays and how JavaScript mistreats them.


**Objects in JavaScript**
In most other programming languages these days, everything is an object. You can see this in current languages like C# where everything inherits from System.Object or in Ruby where you can assign properties and methods to anything at will.


JavaScript is an object oriented language but it&#39;s implementation of objects is a little different from other languages. In JavaScript every object (every thing) is an array - a special array called an "associative array". If you&#39;re a C# developer think `Dictionary&lt;string, object&gt;`. If you&#39;re a python developer then you&#39;re already familiar with this concept. So in JavaScript:


    var x = {
        something: "hi"
    };

    var y = [];
    y["something"] = "hi";


`x` and `y` define the same thing. Both objects will have a `something` property with the value `"hi"`. *Need more proof?*Try the following:


    alert( typeof( new Array() ) );

This will alert `"object"`. Huh?


Yup, an array is just an object with properties named "0", "1", "2" etc. Take a look at this next code block.


    var a = {
        0: "hi",
        1: "there"
    };

    var b = [ "hi", "there" ];


Here `a` and `b` are the same exact thing. [Try it out!][2] 


**Alright, that is insane but why the problem with for/in?**


When you use a for/in on an object in javascript you are looping over the keys in the key/value collection that makes up that object. So doing


    var x = {
        prop1: "test",
        prop2: "test"
    };

    for(var p in x ) { 
        alert(p); 
    } 


This code will alert "prop1" followed by "prop2". Using a for/in doesn't try to treat the object as an array, it enumerates each property (or key) in the object.


Now you should start to see why for/in is generally a bad idea in JavaScript - that is, unless you **want** to loop over the properties of an object.


**Another fun fact**


The for/in loop isn't guaranteed to alert "prop1" before "prop2". The properties will be enumerated in whatever order the interpreter returns them in.


So to summarize, unless you want to enumerate an objects properties, and you don't care about their order, avoid using the for/in loop in your JavaScript code.


  [1]: http://codeimpossible.com/2010/09/20/spot-the-bug-forin-and-javascript/
  [2]: http://beta.jsvudo.com/3f40176
