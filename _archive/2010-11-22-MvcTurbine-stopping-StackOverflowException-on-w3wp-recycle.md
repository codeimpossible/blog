---
layout: post
status: publish
title: 'MvcTurbine, stopping StackOverflowException on w3wp recycle'
slug: "MvcTurbine-stopping-StackOverflowException-on-w3wp-recycle"
---

## I had an issue the other day with MvcTurbine where our application would throw a StackOverflowException whenever the worker process recycled.


I love MvcTurbine. If you're working on a asp.net mvc project then you should take a look at it. Having said that I've run into one pretty strange problem with it.


Another developer noticed that there were a lot of eventlog entries on our build server for our projects w3wp process. After looking into it I found that whenever our CI server ran a build the w3wp.exe process would exit with a StackOverflowException.


Strange.


This wasn't a big deal since it wasn't affecting the overall availability of the application, but it was annoying to the dev team.


In visual studio, every time a file is added or a build is done locally the worker-process is recycled which meant that everytime a build was done locally, or a file was added to the project, each dev got the wonderful "Visual Studio Just In-Time Debugger" dialog. Like I said, not a big issue, just very annoying.


After a bit of googling I found that this is indeed an issue with MvcTurbine but it has a very easy fix: just add the following lines to the class that inherits from `TurbineApplication`:


    protected override void ShutdownContext ()
    {
        CurrentContext = null;
        ServiceLocator = null;
    }
    


[Javier Lozano][1]  - the author of MvcTurbine, explains what&#39;s causing the problem:

&gt; 

Here are some links where you can [see the original post with Javier's answer][2] , take a [look at the code that Javier posted showing the fix in place][3] , and [the StackOverflow question that should provide some more context][4] .


  [1]: http://lozanotek.com/blog/
  [2]: http://mvcturbine.codeplex.com/Thread/View.aspx?ThreadId=219449
  [3]: https://github.com/jglozano/mvcturbine/blob/master/src/Samples/SMExtensions/Mvc/StructureMapTurbineApplication.cs
  [4]: http://stackoverflow.com/questions/3344652/adding-removing-a-file-in-vs2010-causes-webdev-webserver20-exe-has-stopped-worki
