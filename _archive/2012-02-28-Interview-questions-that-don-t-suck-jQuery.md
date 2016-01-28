---
layout: post
status: draft
title: "Interview questions that don't suck - jQuery"
slug: "Interview-questions-that-don-t-suck-jQuery"
---

## Continuing the series of interview questions that don't suck, this time focusing on a nice jQuery/HTML question.


Continuing on in the "Interview Questions That Don't Suck" series, this time I wanted to highlight a nice jQuery/HTML question that I've used in the past.


**The following code binds an event to every `<a />` tags click event. How many problems can you spot in the code below. What are they? Why are they problems?**

    <script type="text/html">
     $('a').each(function() {
       $(this).click(function(e) {
         alert('clicked!');
       });
     });
     </script>

How did you do? Post your answers in the comments!


