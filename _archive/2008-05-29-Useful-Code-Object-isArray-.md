---
layout: post
status: publish
title: 'Useful Code: Object.isArray()'
slug: "Useful-Code-Object-isArray-"
---
When you need to find out the type of an object in JavaScript you would just do the following:

    var x = 1;
    alert(typeof x);

Well what if x were an array? With x as an array a typeof incorrectly returns "object" as the type. Well that is just plain wrong. Enter the isArray() method.

    Object.isArray =  function(o) {
        return o && o.constructor === Array;
    };

Granted it's not as simple as typeof but it does correctly identify array objects and is something every JavaScript developer should have in their [code garage][1] .

  [1]: http://www.hanselman.com/blog/GarageSalesAndGarageSaleQualityCode.aspx
