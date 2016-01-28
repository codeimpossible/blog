---
layout: post
published: true
title: 'Github should select a license for you'
slug: "oss-license"
date: '2013-05-22 23:30 -04:00'
tags: "open-source licensing github"
---

## More and more people are posting code on Github without including a OSS License. It's the art museum way of sharing code. _You can come in and look at my code, but NO PICTURES_.

<a href="http://www.flickr.com/photos/jb912/6972534169/sizes/l/in/photostream/" title="image is copyright -Jeffrey- on flickr" >
    <img src="https://dl.dropboxusercontent.com/u/6291954/6972534169_e525653d06_b.jpg" title="image is copyright -Jeffrey- on flickr" class="attract"/>
</a>

I'm tired of finding cool open source projects on github that I can't use because they don't include a OSS license. It's [infuriating](http://hyperboleandahalf.blogspot.com/2010/05/sneaky-hate-spiral.html).

When you put code out into the world you have two options a) attach some sort of Creative Commons or OSS License to it, or b) don't. If you choose the second option then you're kind of missing the point about making your source public! Releasing code without picking some sort of usage terms means that nobody can use it.

Creating a `LICENSE` file - the community agreed upon way to license your code on Github - is completely optional, and if you actually do include one, you're in the minority.

[An article on The Register](http://www.theregister.co.uk/2013/04/18/github_licensing_study/) says that only about 15% of the projects on github declared a license.

 > According to Williamson, out of the 1,692,135 code repositories he scanned, just 219,326 of them – 14.9 percent – had a file in their top-level directories that identified any kind of license at all. Of those, 28 per cent only announced their licenses in a README file, as opposed to recommended filenames such as LICENSE or COPYING.

I decided to poke around a bit at work to see if I got similar results. _Working on [Ohloh](http://ohloh.net) has it's perks!_ Ohloh currently has 125k github repositories in it's database. Of those, only ~30k had a `LICENSE` or `COPYING` file in their root. So that means that 75% of the code that Ohloh has from github isn't really useable by anyone!

My numbers were a bit higher but I also was using a smaller subset than the guy the article mentions.

Allowing repositories to be created with no license was acceptable when github was focused on collaboration, but with the focus shifting towards getting others to use your code not having licenses on so many repositories is unacceptable. Most of the project owners I've contacted have been really great about adding a license. In fact, a lot of them just reply with "Oops! Shit, I'll add one right now." Why not force them to do this by including the license automatically?

When you create a repo on Github, a `LICENSE` file should be created that contains the BSD license. If you want to overwrite it you can update it yourself. Please Github, let's make this a thing!
