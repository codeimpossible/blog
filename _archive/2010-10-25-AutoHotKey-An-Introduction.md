---
layout: post
status: publish
title: 'AutoHotKey: An Introduction'
slug: "AutoHotKey-An-Introduction"
---

## AutoHotKey is a free, open-source macro-creation app for Windows. It lets you create really powerful macros that respond to keyboard input from within any application. 

Autohotkey is a great, free tool that helps you get more work done in fewer keystrokes - which is always good if you're a programmer. I use AutoHotKey at both home and work to perform some simple (and some complex) text replacement macros. In this post I'll take you through a few simple examples of what AutoHotKey can do to show why I think this is an essential item for your developer batman utility belt.

You can download AutoHotKey from its website, [here][1] .

**The really, really basic example - replacing text: **
Here is a sample script that I use to write out my work email address

    
    :*:@work::jbarboza@wakefly.com
    


It looks a little confusing but trust me, it's really simple. The * tells AHK that this script is an "instant replacement" and AHK should not wait for the spacebar to be pressed. "@work" as you've probably guessed is the trigger and "jbarboza@wakefly.com" is the replacement text. Note: AHK doesn't care about the @ symbol in `@work`, it's only there for consistency with my other email triggers.

**Another simple example - fixing common misspellings**
I suck at spelling. I make spelling mistakes **all the time**. These script lines show how I auto-correct a few of my more common spelling mistakes.

    
    :*?:.cm::.com 
    :*?:.ocm::.com
    


**AutoHotKey can also run applications:**
AHK isn't just for replacing text. The built-in scripting language can do a multitude of things, from manipulating selected text, gathering user input via prompts and using it in script output, even automate applications.

    
    #f::Run Firefox
    


This script will run Firefox when you press [Windows Button] + f. The # symbol in AHK is a shortcut for the windows key. You can use other shortcuts like ^, and ! for CTRL and ALT respectively.

This is just a small sampling of what AHK can do, if you're interested in learning more about the AHK scripting language check out the [HotKey Introduction in the AHK Documentation][2] , the [AHK articles on Lifehacker][3] , or [the AutoHotKey wikipedia page][4] .
  [1]: http://www.autohotkey.com/download/
  [2]: http://www.autohotkey.com/docs/Hotkeys.htm
  [3]: http://lifehacker.com/tag/autohotkey/
  [4]: http://en.wikipedia.org/wiki/AutoHotKey
