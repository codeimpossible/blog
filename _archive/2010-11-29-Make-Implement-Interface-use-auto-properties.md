---
layout: post
status: publish
title: 'Make "Implement Interface" use auto properties'
slug: "Make-Implement-Interface-use-auto-properties"
---

##This week I'm finding that there is a lot less pain involved in getting Visual Studio 2010 to do things my way than I thought - like no pain at all.

I'm refactoring some legacy code this week; a task that would normally be a total PITA but thanks to Visual Studio's built-in refactoring tools it's going much easier than expected. The one tool that I'm a huge fan of is the "Implement Interface" context menu item.


To see what I mean create a new class, something simple...

    public class MySimpleClass : IDisposable
    {
    }

yeah, like that.


Now move your cursor over "IDisposable" and press ctrl+period. Selecting "Implement Interface" from the menu that appears will automagically insert all the properties and methods needed to make your class conform to the `IDisposable` interface.


It's very handy, but it generates the properties in a way that I'm not particularly a fan of:


    public object SomeProperty
    {
        get
        {
            throw new NotImplementedException();
        }
        set
        {
            throw new NotImplementedException();
        }
    }
    


Instead I'd like to have this code generated using auto properties so that it looks like this:


    public object SomeProperty { get; set; }



I initially thought I would have to modify some built-in T4 template but thankfully it's much, much easier than that. Apparently the visual studio team took full advantage of the snippet feature when they added it because nearly every refactoring tool is editable via a `.snippet` file. You can find the snippets that visual studio uses for C# in the following directory: `%programfiles%\Microsoft Visual Studio 10.0\VC#\Snippets\1033`


The file you're looking for is in the `Refactoring` directory and is named `PropertyStub.snippet`. Open this guy up in your favorite text editor - don't worry it's just xml - and find the part that looks like this:

    <![CDATA[$signature$
    {
        $GetterAccessibility$ get 
        { 
            $end$throw new $Exception$(); 
        }
        $SetterAccessibility$ set 
        { 
            throw new $Exception$(); 
        }
    }]]>
    


Replace this xml with the xml below and you'll be good to go after a quick restart of Visual Studio!

    <![CDATA[$signature$ { $GetterAccessibility$ get; $SetterAccessibility$ set; }]]>

