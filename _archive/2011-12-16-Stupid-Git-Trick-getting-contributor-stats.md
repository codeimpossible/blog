---
layout: post
status: publish
title: 'Stupid Git Trick - getting contributor stats'
slug: "Stupid-Git-Trick-getting-contributor-stats"
---

## Getting contributor stats for your non-public project can be a real pain, that is, unless you're using git!


We use [github][1]  for all of our source code and documentation on the [Ohloh][2]  team. We&#39;ve even got a few [public open source][3]  projects up there that you should check out, but I&#39;ll talk about those another time.


Earlier this week I tweeted my YTD commit stats from my new job:



And I was asked how I came up with those numbers, which made me realize two things:


 1. git is awesome
 2. those numbers are wrong!

## Git is awesome ##
Git **is** awesome, and for a of other reasons that I&#39;ll cover in some future posts. Git is awesome right now because it collects stats. Not as a main feature, not really anyway, but as a by product of you and your team pushing code around.

Every commit in git is logged so git knows what the heck is going on at any given moment. Git is like that kid in college or high school that was so awesome at taking notes that he was able to buy a cannondale super v using the money he made selling copies of his notes to other people.


Git could probably afford to buy Amazing Fantasy issue 15 (first appearance of spider man for the non-nerds), git is **that** good at it.


Getting your commits from a git repo is pretty damned easy:


    git log --author="Jared Barboza" --pretty=tformat: --numstat
    


That line will ask git to go into its log and pull out the commits that you've made and format them using the "numstat" formatting, more on this in a moment. The output will look like the example below, with the first column displays the number of additions (lines you added) and the second number is the number of deletions (lines removed).


    5       8       public/javascripts/main.js
    2000    0       public/javascripts/jquery/jquery.cycle.all.js
    10      9       public/stylesheets/layout.css
    


Awesome, let&#39;s see if we can a nicer looking total out of this. Now, in order to do this we&#39;re going to need to do some terminal magic with [gawk][4] .


If you've never used gawk before this might be really strange looking to you but it's pretty easy to grok. Gawk allows us to run "code" via the terminal and uses any text piped to it as arguments. So we can do this:


    git log --author="Jared Barboza" --pretty=tformat: --numstat | \
    gawk '{ add += $1 ; subs += $2 ; loc += $1 - $2 } END \
    { printf "added lines: %s removed lines: %s total lines: %s\n",add,subs,loc }' -
    


This will pipe (more about piping in the next section) the output from the `git log` command into gawk which will use each line as a new set of arguments and will keep a running total of the additions (first argument) , deletions (second argument) and the difference between the two (the "true" lines of code I&#39;ve commited).


After gawk is done we'll see this result on our terminal:


    added lines: 2015 removed lines: 17 total lines: 1998
    


For those of you who are linux termnial n00bs like me, the `\` in the terminal command above is a line continuation character. It lets us tell the terminal that the command is continued on the next line.


The `|` character tells the terminal to take the output from the command on the left and feed it to the command on the right, this is called "piping".


For all my Windows peeps, you can grab [grep][5]  and [gawk][6]  for Windows to get this to work.


Now, back to the stats. They&#39;re great but they are not really accurate are they? I mean it&#39;s counting a jquery library ([jQuery Cycle][7]  by Mr. Amazing himself, [Mike Alsup][8] ) which isn&#39;t my code so I don&#39;t want that to be part of my count. Which brings me to my next point.

## The numbers I posted are completely wrong! ##

When I posted those numbers on the twitters I totally ignored the fact that I was counting files that I didn&#39;t write. So I needed to figure out how to remove certain lines from the result *before* it gets passed into gawk.


I found grep has a `-v` option that tells it to ignore lines that match the text supplied. So I can use the following in the "pipeline" (a chain of piped commands) to trim out the unwanted code file.


    grep -v public/javascripts/jquery
    


When that it combined with the script from before I end up with:


    git log --author="Jared Barboza" --pretty=tformat: --numstat | \
    grep -v public/javascripts/jquery | \
    gawk '{ add += $1 ; subs += $2 ; loc += $1 - $2 } END \
    { printf "added lines: %s removed lines : %s total lines: %s\n",add,subs,loc }' -
    


Which will print out


    added lines: 15 removed lines: 17 total lines: -2
    


Now this is a much better count. We can see that I've actually removed code from our codebase which is always a good thing.

## The "Official Stats" for my first 3 weeks ##



So now that we have a much better way of determining commit stats, I re-did all my stats for the last three weeks. I used grep commands to break out the commit stats even more e.g. using `grep /test` and `grep /public/javascripts` to get the stats for unit tests and javascript.


**Unit Tests:** added lines: 696 removed lines: 107


**Controllers:** added lines: 43 removed lines: 50


**Models:** added lines: 71 removed lines: 77


**Helpers:** added lines: 15 removed lines: 7


**Views:** added lines: 221 removed lines: 143


**Javascript:** added lines: 1112 removed lines: 927


**Css:** added lines: 416 removed lines: 220


**Project Total:** added lines: 2574 removed lines: 1531


 - 6.9% of removals and 27% of additions were unit tests
 - 60.5% of removals and 43% of additions were javascript



  [1]: http://github.com
  [2]: http://ohloh.net
  [3]: http://github.com/blackducksw
  [4]: http://gnuwin32.sourceforge.net/packages/gawk.htm
  [5]: http://www.wingrep.com/features.htm
  [6]: http://gnuwin32.sourceforge.net/packages/gawk.htm
  [7]: http://jquery.malsup.com/cycle/
  [8]: http://twitter.com/malsup
