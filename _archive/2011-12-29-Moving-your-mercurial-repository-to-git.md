---
layout: post
status: publish
title: 'Moving your mercurial repository to git'
slug: "Moving-your-mercurial-repository-to-git"
---

## I said it last week, git is awesome. I've been using git and github at work and I like it. A lot. I like it so much I've decided to move all my bitbucket repos over to github.




Moving source code is easy. You can do it with a couple of mouse clicks, a commit and be done in about 5 minutes if you really wanted to. But if you did, you'd be missing the most important part: the history.


The greatest thing about source control is that it gives you a history for your project. You can see how a project has grown from one commit to the next. You can see who the heavy hitters are and who built that kick ass feature you love.


However, this becomes a problem when you talk about moving your source code from one repository vendor to another. You have all this history... this is the stuff that I really wanted to move over.


Thankfully, in the case of moving from Mercurial to Git, a programmer by the name of Cosmin Stejerean - check out his blog at [offbytwo.com][1] , it's a great read - built a hg import module for git.


The hg to git importer ([git-hg][2] ) is a bunch of shell scripts that interact with mercurial and sort of "replay" your mercurial commits into a new, empty git repository that git-hg creates on your machine. 


## Installing git-hg ##



Installation of git-hg is simple but you'll need to have hg and python installed and in your `PATH`.


You should do a dump of your `$PATH` variable to make sure you have these apps there or if you're lazy like me you can just try to run each app.


    $ hg --version
    $ Mercurial Distributed SCM (version 1.9.1)
    $ (see http://mercurial.selenic.com for more information)
    
    $ Copyright (C) 2005-2011 Matt Mackall and others
    $ This is free software; see the source for copying conditions. There is NO
    $ warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    
    $ python --version
    $ Python 2.7.2+
    


Now all that is missing is git-hg. Clone the git-hg repo with git and install the submodule.


    $ git clone http://github.com/offbytwo/git-hg
    
    $ cd git-hg
    $ git submodule update --init
    


After the submodule is registered add git-hg into the PATH. I usually copy the necessary files to `/usr/local/bin`.


    $ cp git-hg/bin/git-hg /usr/local/bin/git-hg
    $ cp git-hg/fast-export /usr/local/fast-export
    


## Porting your first repository ##



    $ git-hg clone http://bitbucket.org/codeimpossible/artigo artigo-git
    


Git-hg will clone the repo, this can take a while depending on how many commits your repo has. Once it's done you can add a new remote for your github repo and push it up.


    $ git remote add github http://github.com/codeimposible/Artigo.git
    $ git push github master
    
    ... enter credentials here ...
    


And it's as easy as that! I realize this was a linux/mac only walkthrough, if anyone has steps or tools that they use to accomplish the same thing on Windows please post in the comments, I'd like to help as many people as possible move over to the 'hub.


Also, I'm no linux expert so if I made really stupid mistakes let me know so I can fix them up!

  [1]: http://offbytwo.com
  [2]: http://github.com/offbytwo/git-hg
