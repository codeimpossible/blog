---
layout: post
status: publish
title: 'AutoHotKey scripts for developers'
slug: "AutoHotKey-scripts-for-developers"
---

## This week I'll post some useful AutoHotKey scripts that I use to make every-day coding easier.


If you're not familiar with AutoHotKey yet, I've [blogged about it before][1]  and I would recommend giving that post a read. For the rest of you I thought I would post some useful AHK scripts that I have come across.


**The most important AHK script ever. **
Disabling Caps-lock. I constantly fat-finger the caps-lock button. With this script, I never have to worry about it again.

    SetCapsLockState, AlwaysOff

**Generate a "safe" jQuery code block**
This script will generate a self-calling anonymous function that scopes jQuery and protects your code from other possible interference from other javascript frameworks. You can [read more about it here][2] .

    ;insert a self-calling anonymous method to scope jQuery calls
    :*R0:{jqsafe}::
    (
    (function($) &123;&123;} 
        $(function() &123;&123;}
        // ...code to run on dom ready

    {Left}{Left}{Left}{Left}{&125;&125;);`n{&125;&125;)(jQuery);
    )
    return

**Include jQuery from the Google CDN**
This script will turn `{jqgoog#.#.#}` into a `&lt;script&gt;` tag pointing to that version of jquery, specified by the #.#.#, hosted on googles cdn. `{jqgoog1.4.2}` will become: *Line breaks added for readability*

    &lt;script 
    type=&rdquo;text/javascript&rdquo; 
    src=http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js&gt;&lt;/script&gt;

`{jqgoog1}` will become: *Line breaks added for readability*

    &lt;script 
    type=&rdquo;text/javascript&rdquo; 
    src=http://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js&gt;&lt;/script&gt;

Here is the script.

    ; Include jQuery from google cdn
    ~{::
    Input, UserInput, I V L13 C, {&125;&125;, jqgoog*,
    if ErrorLevel = NewInput
        return
    Test := InStr( UserInput, "jqgoog" )
    Len := StrLen( UserInput ) + 2

    if Test &gt; 0:
    {
    jqVer := RegExReplace( UserInput, "jqgoog", "" )
    SetKeyDelay, -1  ; Most editors can handle the fastest speed.

    Loop %Len%
    {
    Send, {backspace}
    }

    Send, &lt;script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/%jqVer%/jquery.min.js"&gt;&lt;/script&gt;
        return
    }
    return

**Automate writing simple database connection code in C#**
This script will turn `{atwood}` into a block of C# that can connect to a database and issue a query.

    :*R0:{atwood}::
    (
    var connString = ConfigurationManager.ConnectionStrings["connection"].ConnectionString;
    var query = @"";
    using(var conn = new SqlConnection(connString))
    {&125;&125;
        conn.Open();
    using( var cmd = new SqlCommand( query, conn ) ) 
    {&125;&125;
        
    {&125;&125;
    {Left}{&125;&125;
    )
    return

    Here's the output:


    var connString = ConfigurationManager.ConnectionStrings["connection"].ConnectionString;
    var query = @"";
    using(var conn = new SqlConnection(connString))
    {
        conn.Open();
        using( var cmd = new SqlCommand( query, conn ) )
        {

        }
    }

Enjoy!


  [1]: http://codeimpossible.com/2010/10/27/autohotkey-an-introduction
  [2]: http://codeimpossible.com/2010/01/13/solving-document-ready-is-not-a-function-and-other-problems
