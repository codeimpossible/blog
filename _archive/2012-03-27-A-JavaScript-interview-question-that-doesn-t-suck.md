---
layout: post
status: publish
title: "A JavaScript interview question that doesn't suck"
slug: "A-JavaScript-interview-question-that-doesn-t-suck"
---

## Starting a series of blog posts where I'll show off some interview questions that provide insight and don't totally suck.

At a previous job I was one of the more senior guys on the team and we were expanding heavily. Which meant that I was in a lot of interviews. Problem was, our interview process sucked. We rarely had people code in interviews, we never had them look at code, it was awful. So I worked with the rest of the devs and we revamped the entire process. It was awesome!

It's been a while but I stumbled across the questions in an old google doc and thought I would share them.


Questions that involve having people look at code and critique it are my favorite questions to ask in an interview. You get to see what, if any, biases they have when reviewing code and you can see how much of the language they understand.


**The following code is supposed to loop over a collection, alerting each item. If any item in the collection equals the number 4, then the item is divided by 4 and the result is alerted.**


**How many problems can you spot in the following code sample? What are they? Why are they problems?**


    var collection = new Array(1,2,3,"4",5);
    for(var i = 0; i <= collection.length; ++i) {
      alert(collection[i]);
      if( collection[i] == 4 ) {
        alert(collection[i] / 4);
      }
    }

So, how did you do? Post your answers in the comments!


