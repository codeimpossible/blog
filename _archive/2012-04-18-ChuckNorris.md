---
layout: post
status: publish
title: 'ChuckNorris'
slug: "ChuckNorris"
---

## @JohnBubriski mentioned a project from the ChuckNorris suite of tools and I found myself browsing their git repo


Looking through the git repo for [ChuckNorris][1] , seeing the amazing number of tools that  [Dru Sellers][2]  and [Rob Reynolds][3]  have turned out leaves me thinking one thought:


![][4] 


If you don't know what ChuckNorris is, a lot of people call it a framework but it's more like a suite. There are, at the time I wrote this, about eight tools that target specific aspects of the development process.  From database migrations, continuous builds, to deployment. ChuckNorris has a story for pretty much everything.


You don't have to use the tools together, they each work just as well separately, but when you combine them they make your life insanely simple.


You can listen to a [episode of Hanselminutes where Scott interviews Rob about the framework][5] . It&#39;s a great episode, it reveals a lot of the inside details on how ChuckNorris came about.


To show my love for ChuckNorris I thought I would write up a few of the tools that I've either used in the past or have thought were interesting.


[RoundhousE][6]  is a database migration tool, like db:migrate in Rails (or the new Entity Framework migrations). I&#39;ve used this in the past and it&#39;s very awesome.


[WarmuP][7] , which is what [@JohnBubriski][8]  and I were talking about, is a project bootstrapper. Setup your project directories, templates, dependencies once and WarmuP will get them from source control and perform a token replacement on them. Pretty cool stuff.


[UppercuT][9]  is a templated build system written on top of nant. I&#39;ve used this in the past as well, it sports a ton of extensibility points. Pretty much every step in the build process has Pre, Post or Replace hook so you can make it do **anything** and you can make it do anything in a matter of minutes.


Are there any OpenSource libraries/suites that you guys use that constantly surprise you with the sheer amount of features that they have? Give them a shoutout in the comments!


  [1]: https://github.com/chucknorris
  [2]: https://github.com/drusellers
  [3]: https://github.com/ferventcoder
  [4]: http://dl.dropbox.com/u/6291954/bluesbrothers.jpg
  [5]: http://castroller.com/Podcasts/Hanselminutes/2712218
  [6]: https://github.com/chucknorris/roundhouse
  [7]: https://github.com/chucknorris/warmup
  [8]: http://www.johnnycode.com/blog/
  [9]: https://github.com/chucknorris/uppercut
