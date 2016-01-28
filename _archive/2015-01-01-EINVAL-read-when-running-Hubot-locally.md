---
layout: post
title: 'Work-around for EINVAL, read when running Hubot locally'
published: true
tags: "opensource hubot troubleshooting"
---

**tl;dr - just delete the `.hubot-history` file in your local hubot install directory**

Earlier today I was trying to test out pull request submitted by someone for our [hubot-trello](https://github.com/hubot-scripts/hubot-trello) plugin. Testing hubot locally is pretty straightforward:

 - install a hubot instance _somewhere_ on your machine (we often use a subdirectory named `test`)
 - use `npm link` to make the files in your plugin source directory available to the new hubot install
 - do any environment variable setup you need to do
 - run the new hubot

For those of you that want to see this as shell commands:

{% codeblock lang:bash %}
$ hubot --create test # create a new hubot in the "test" directory
$ npm link && cd test && npm link hubot-trello # link our current hubot-trello source
$ cd ../ && ./.hubot-trello # start the test hubot with the trello env vars
{% endcodeblock %}

This usually works great. Until today of course. Today when I spun up the new hubot install it ran fine the first time, but failed each time after that. The logs weren't very helpful.

{% codeblock lang:bash %}
[Thu Jan 01 2015 15:06:42 GMT-0500 (Eastern Standard Time)] DEBUG Loading adapter shell
[Thu Jan 01 2015 15:06:42 GMT-0500 (Eastern Standard Time)] ERROR Error: EINVAL, read
{% endcodeblock %}

What the _hell_ is `EINVAL, read`? Googling gave me nothing. My guess was that something was trying to read a file, and couldn't for _some reason_. Using this I started a very technical troubleshooting process of throwing `console.log` statements everywhere to see how far into the hubot source I was getting before the error happened.

After debugging like a professional computer person, I found that the error was coming out of a npm package named "readline-history". The built in shell adapter that comes with Hubot uses this library to list out all the commands you typed in when you enter the command `history`.

Honestly, this seems like overkill - do people really care if the Hubot shell remembers what they typed in the last time they ran it? Also, don't most terminals remember stuff for you during your current session? Seems like this whole thing just isn't needed.

![louie, wtf](https://dl.dropboxusercontent.com/u/6291954/gifs/louie9.gif)

Anyway, getting back to the problem. I'm not sure why this library is throwing the `EINVAL` error mentioned above, but when I deleted the `.hubot-history` file in the root of test hubot directory it started up fine. So there you have it. If you're testing hubot against a newer hubot version and you see this error, try deleting that file and hopefully you can get back to making your hubot plugins more awesome!
