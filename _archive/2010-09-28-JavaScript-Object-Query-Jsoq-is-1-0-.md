---
layout: post
status: publish
title: 'JavaScript Object Query (Jsoq) is 1.0!'
slug: "JavaScript-Object-Query-Jsoq-is-1-0-"
---
I've finally pushed one of my side-projects, the JavaScript Object Query library to 1.0! I'm pretty psyched since I've been meaning to do this ever since [I blogged about it back in February][1] .

You can download the 1.0 release [here (minified)][2]  and [here (normal)][3] .

**What is Jsoq?**
Jsoq is a port of Linq to Objects to JavaScript. Under the covers jSoq is a bunch of wrapper code for dealing with arrays in JavaScript.

**Features for v1.0**

     - Doesn't mess with "$" variables so it works with jQuery, Mootools, etc.
     - using select() you can have Jsoq return only certain members from each object in it's query set
     - use built-in filter methods first(), last(), skip(), take() or roll your own using where()
     - ability to add functionality to your returned object via the extendEach() method
     - you can use $q as a shortcut for the jsoq.From() method
     - inner and left joins are now supported with the leftJoin() and innerJoin() methods

**A very basic code example**
Below is what I would call the common jsoq use case. The code below will find the first item that has a `value` property with a length greater than 3, and return an object containing its `id` property.

    var somearray = [
        { value: "one", id: 1 }, 
        { value: "two", id: 2 }, 
        { value: "three", id: 3 }, 
        { value: "four", id: 4 }
    ];
    
    var result = $q(somearray)
                        .where(function(item) {
                            return item.value.length &gt; 3;
                        })
                        .first()
                        .select("id");
    
    alert(result.id); // alerts "3"

For a full list of features and more code examples check out [the documentation on the official Jsoq wiki][4] , it's a bit sparse right now but I'll be adding more content in the near future.
  [1]: http://codeimpossible.com/2009/02/10/introducing-the-javascript-object-query-library/
  [2]: http://bitbucket.org/codeimpossible/jsoq/downloads/jsoq.1.0.min.js
  [3]: http://bitbucket.org/codeimpossible/jsoq/downloads/jsoq.1.0.debug.js
  [4]: http://bitbucket.org/codeimpossible/jsoq/wiki/Home
