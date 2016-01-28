---
layout: post
title: 'Screw it, run this file!'
slug: "screw-it-run-this-file"
date: '2012-09-28 00:00:00 -04:00'
tags: "development devx developer-experience practices"
---
## Ever experienced a nightmarish environment setup and thought "If this project is this hard to setup, how hard is it going to be to work on?". Yeah, me too.

Ashic Mahtab was experiencing something similar as well:

![tweets](http://content.screencast.com/users/codeimpossible/folders/Jing/media/dd6ac330-4a77-41e9-b6eb-4deac03af346/2012-09-26_2006.png)

I've worked on plenty of codebases whose setup story sounded like it was ripped from a rejected plotline for Indiana Jones: The Search For The Next Release.

1. Pull the code from source control
2. install these 23 dependencies, from these 33 websites, make sure you get the right versions!
3. patch this library and compile it from source, because we depend on it working a certain way
4. install the database server
5. run the initial database creation script, which is massive and hasn't been tested
6. now, run these 44 migration scripts to get up-to-date with source control
7. open the source in the IDE and compile
8. run the unit tests

_WHEW!_ I'm tired just writing that list out. Did you notice how long it took us to just get the code opened up in the editor? Thats rediculous. Especially when you consider what it takes to start working with the [Rails](http://github.com/rails/rails) project:

    $ bundle install
    $ rake test

With those two commands I've installed all the project dependencies, and run the test suite. Every open source project needs a single file that new people can run to do all of the uncool, awful setup tasks that the project requires. Things like installing and configuring dependencies or creating and seeding a database.

Similarly, and IMO the best example of this, is the [Code52](http://code52.org) project [Jibbr](http://github.com/code52/jibbr). Here's the command to get everything necessary:

    $ ./build.cmd

This file should be an example of what every project should do to eliminate barrier to entry for committers to their projects. Don't sit around on a project that has 200 steps to setup and then wonder why no one is contributing!

### Screw it, run this file!

This is a concept that I felt really strongly about when I wrote [Artigo](http://github.com/codeimpossible/Artigo) (my blog platform that is no more). Yeah no one used it and it never went anywhere but if you pull Artigo out of github today, there is only one rake command you need to run:

    $ rake artigo:first_time

BOOM. You're setup. This one task cleans your db, migrates it, seeds it and then *runs the test suite*. If your project isn't this simple to get up and running then nobody is going to work on your stuff.

Dev experience for .Net developers is getting a lot better, with Nuget and its new(er) feature to auto-download dependencies on build, devX is finally getting pushed to the foreground.

But there is still more to be done. And it has to be done by us. We need to add unit tests to our projects, take the time to write a [single powershell script to setup IIS](http://mikefrobbins.com/2012/05/15/create-a-new-iis-website-with-powershell/) and migrate our databases with [RoundhousE](http://github.com/chucknorris/roundhouse). We don't have to use these specific solutions but we need to use _some_ solution.

In short, look at [Rails](http://github.com/rails/rails), [Jibbr](http://github.com/code52/jibbr) and dozens of other projects to see how they are solving this problem and then _solve it_.

Your future contributors/teammates will thank you for it!
