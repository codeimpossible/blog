---
layout: post
status: publish
title: 'Useful Code: Object.Enumerate'
slug: "Useful-Code-Object-Enumerate"
---
I write a lot of javascript at my job and I often find myself in a horrible debugging situation where something isn't working right and I need a way to find out what properties and methods an object has quickly. I've found the following code very helpful in determining this.


    Object.Enumerate = function(o,t) {
        var type = typeof t === 'undefined' ? null : t;
        for(var key in o)
        {
            var value = o[key];

            // if the user did not pass a
            // type then "alert" each member
            if(type == null ||
                typeof value === type)
            {
                alert( key + " is a " +
                    (typeof value));
            }
        }
    };


This is a really simple method that will take an object and (optionally) a string representation of a type ("number", "object", "function" or "string") and will display the members that match those types (or if no type is passed it will display each member and it's type).

    //basic object for testing
    var x = {
        int: 1,
        str: "",
        func: function() {

        },
        obj: {
            str2: ""
        }
    };

    // alerts each member of 'x' and it's type
    Object.Enumerate( x);

    //alerts "func is a function"
    Object.Enumerate(x, 'function');

This code is really similar to how prototype generates JSON information and can easily be modified to return various types of data back to the requestor.

Note this code isn't perfect, for instance it doesn't account for Arrays which are typed as objects in javascript. Also it doesn't do any sort of recursion so it will only return top-level members.

But even so this is still the most used script in my library and I make sure I have it in all my projects. Hopefully it helps someone else.

