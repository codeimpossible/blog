---
layout: post
status: publish
title: 'Reasons to use PowerShell instead of CMD'
slug: "Reasons-to-use-PowerShell-instead-of-CMD"
---

## I stopped using cmd.exe in favor of PowerShell almost a year ago. Here's a quick list of things that made me drop CMD forever.


First, I guess I should say that there isn&#39;t anything necessarily **wrong** with the command line in windows. It&#39;s the &#39;ol standby for a lot of windows users. It&#39;s looked the same since 1986, it has the same familiar commands and you can use TrueType fonts if you want to be a rebel.


I.R.O.N.

Powershell is built on top of the .Net Framework which means that [any assembly that is compiled with .net can be brought into powershell][1]  and used to automate/extend the console.


Say goodbye to horrible .bat files in your projects. just create an ShellAutomation.dll in your project and write some powershells scripts and you're good to go!


"ls", "pwd"

Two letters: "ls". The PowerShell command window has support for a ton of the *nix shell commands, no more 'ls is not a command or batch file' errors! This should be enough to get you to switch!


It has modules!!!

[PsGet][2]  is a package manager for PowerShell (remember when I said that [everything needs a package manager][3] ?). You can choose from a ton of modules off their directory and PsGet will isntall them for you.


Git is better in powershell

[Posh-Git][4]  is a PowerShell script for, you guessed it, git that has makes me love working with git more than I ever could on linux/mac. The major selling point? contextual tab-completion. It&#39;s freaking awesome. Git on all other platforms has been put on notice. 


Easy automation

PowerShell is a pretty easy language to grok. I&#39;ve only used it in passing to automate really tedious tasks, but if I can throw a workable script together in 10 minutes then the language **has **to be easy to understand!


Awesome output commands

clip is a command that copies whatever is "piped" to it to the clipboard. Yeah, no more right-clicking on the cmd window icon, selecting "select all" and pressing enter. Just pipe the output!


    $ c:\> ls | clip


The directory listing for my c:\ root would be put into the clipboard, ready to be pasted into a blog post, email or message-board twitter flame.


out-gridview is a similar command but instead of putting the piped data into the clipboard, PowerShell will show a WPF-style data grid view of the data.


So that's my list, if you're using powershell already let me know what you like most about it in the comments. If I've forgotten something obvious let me know!


For more resources on powershell, check out the [Hey!, Scripting Guy][5]  blog as well as [BeefyCode][6]  blog (what an awesome name for a blog). Until next time.


  [1]: http://www.dougfinke.com/blog/index.php/2010/08/29/how-to-load-net-assemblies-in-a-powershell-session/
  [2]: http://psget.net/
  [3]: http://codeimpossible.com/2011/11/29/Installing-the-Package-Control-plugin-for-Sublime-Text
  [4]: https://github.com/dahlbyk/posh-git/
  [5]: http://blogs.technet.com/b/heyscriptingguy/archive/2011/06/18/top-ten-favorite-powershell-tricks-part-1.aspx
  [6]: http://www.beefycode.com
