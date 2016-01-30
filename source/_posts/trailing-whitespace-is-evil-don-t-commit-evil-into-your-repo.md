---
layout: post
status: publish
title: "Trailing whitespace is evil. Don't commit evil into your repo."
slug: "Trailing-whitespace-is-evil-Don-t-commit-evil-into-your-repo-"
date: 2012/04/02 00:00:00
categories:
- development
tags:
- source-control
- best-practices
---

### Lately, I've beeen working on a lot of projects with different people/languages/editors, most of us were new git'ers and each project had a real problem with trailing whitespace.

**Fred and Tim**

It all starts out innocently enough. Fred, a mid-level developer at InfoTech Systems opens up his IM client and fires off a chat to a co-worker: "Hey Tim, can you check out my pull request? I'm done with the ratings feature and think we should merge it in".


Tim, a Senior Software Developer, has been writing code since the mid 80's. He originally started out contributing to the Gnome project and then bounced around the corporate world, Lotus, Microsoft, Oracle but now works with Fred at a small startup where they and five other developers are trying to build the next great cat-picture rating website - CatR.


"Ok, let me CR it", CR is the teams short-hand for Code Review. Even though most of the team are experienced programmers every feature and bug fix commit must be reviewed by another developer.


Fred is the "new guy" on the team, this is his third day at InfoTech and he's submitting his first feature enhancement. Even though Fred has 8 years of coding under his belt, he still gets a little nervous when someone reviews his code.


"Hmmmmm" Tim types back. *Ugh, This can't be good* Fred thinks.


"This git diff looks weird. How many lines did you change in the main stylesheet?"


"Only 2 why?" Fred types back, starting to get a bit defensive. He takes a few quick, deep breaths and calms down.


"The diff is lighting up like a christmas tree. Check your diff locally."


Fred opens up a terminal and does a quick check to see what Tim is talking about:


    git diff master product-ratings-feature


*"Crap!"* the word jumps instantly into Freds mind. He can see immediately what Tims talking about. There a bunch of lines where the only thing different is the whitespace at the beginning or end of the line.


"Ugh, yeah I see it" Fred types back.


"Well, we can't pull this in yet dude, we only want to have diffs show what we actually changed so if something goes south in the future we're not scratching our heads looking at a all whitespace commit. Go back and check your whitespace settings, re-save and let me know when I can review it again."


"Great, guess I've got some more work to do" Fred says to himself. He lets out a fairly audible sigh, opens up Visual Studio and starts typing away at the keyboard.

**The moral of the story**



Trailing whitespace issues can cause a lot of problems when they get into your repository. It leads to falsey diffs which claim lines have been changed when in fact the only thing that changed was spacing.


This can make finding what actually changed in a file later on in the development cycle next to impossible. Most open source project leads know this and a lot of them will reject pull requests that fail to trim whitespace (or have other


A lot of IDEs and text editors have options to configure trailing whitespace (SublimeText make this insanely easy) but Visual Studio, amazingly, has no option for this.


## How to Remove Trailing Whitespace on save in Visual Studio

 1. Open visual studio (yep)
 2. In the menu select Tools -&gt; Macros -&gt; Macros IDE (yeah, we're opening **another** IDE)
 3. Expand "My Macros" in the Project Explorer (usually in the right-hand side of the window)
 4. Double-Click the EnvironmentEvents module (yep, it's VBA in all it's glory)
 5. Paste the code below just after the "Automatically generated code" region

        Private Sub DocumentEvents_DocumentSaved(ByVal document As EnvDTE.Document) _
	        Handles DocumentEvents.DocumentSaved
	        Dim fileName As String
	        Dim result As vsFindResult

	        Try
	            ' Remove trailing whitespace
	            result = DTE.Find.FindReplace( _
	                vsFindAction.vsFindActionReplaceAll, _
	                "{:b}+$", _
	                vsFindOptions.vsFindOptionsRegularExpression, _
	                String.Empty, _
	                vsFindTarget.vsFindTargetFiles, _
	                document.FullName, _
	                "", _
	                vsFindResultsLocation.vsFindResultsNone)

	            If result = vsFindResult.vsFindResultReplaced Then
	                ' Triggers DocumentEvents_DocumentSaved event again
	                document.Save()
	            End If
	        Catch ex As Exception
	            MsgBox(ex.Message, MsgBoxStyle.OkOnly, "Trim White Space exception")
	        End Try
	    End Sub


Now save your new macro and whenever you save a file in Visual Studio this will run and trim all the trailing whitespace.


## How to Remove trailing whitespace on save in Sublime Text 2

 1. In Sublime Text, open up the preferences menu and select "File Settings - User"
   - this is important because if you use the "Default" settings, they may be overwritten when you update Sublime Text to a new version.
 2. Scroll down until you see `"trim_trailing_white_space_on_save"`, set this option to `true`
 3. Save
 4. Profit



## Get Git to help you out

In the example above Fred could have saved himself a lot of time if he ran one command:


    mv .git/hooks/pre-commit.sample .git/hooks/pre-commit



This file has a check (on the last line) that will fail any commit when there are whitespace errors. It's not enabled by default so you have to remove the `.sample` from the file name to get git to run it.

## After all this, what should I do next? ##

High-five Yourself!!!


![][1]


  [1]: http://dl.dropbox.com/u/6291954/MnEIl.gif
