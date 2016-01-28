---
layout: post
status: publish
title: 'Installing mercurial on Ubuntu 6.06'
slug: "Installing-mercurial-on-Ubuntu-6-06"
---

## Over thanksgiving break I dug an old linux pc out of our basement and installed began setting it up as a continuous integration machine for some personal projects.


Installing git and svn on Ubuntu 6.06 is incredibly easy but getting mercurial installed required a few more steps. Here&#39;s what I did to get it running. *Update: I'm keeping the list of steps here to help anyone thats stuck but if you have 6.06 and an internet connection then you should skip to the end of this post for the pain free installation steps.*


 1. Mercurial needs python &gt;= 2.4 to run, make sure it&#39;s installed. `$ sudo apt-get install python`
 2. You&#39;ll also need the Python C headers and a C compiler. `$ sudo apt-get install build-essential gcc python-dev`
 3. If you want to generate the documentation you&#39;ll need AsciiDoc and XmlTo as well. `$ sudo apt-get install asciidoc xmlto`
 4. download Mercurial from the [dapper package repo][1] . `$ wget REPO_URL`
 5. Install Mercurial! `$ sudo dpkg -i mercurial_0.7-8_i386.deb`

Altogether this would look like:

    $ sudo apt-get install python
    $ sudo apt-get install build-essential gcc python-dev $ sudo apt-get install asciidoc xmlto
    $ wget http://mirrors.rit.edu/ubuntu//pool/universe/m/mercurial/mercurial_0.7-8_i386.deb
    $ sudo dpkg -i mercurial_0.7-8_i386.deb

**Installing Mercurial with no headaches**
After doing all this I found out than I could have just [upgrade my installation of Ubuntu to 8.04][2]  and then run `$ sudo apt-get install mercurial` to install the latest version pain free. Doh!


  [1]: http://packages.ubuntu.com/dapper/i386/mercurial/download
  [2]: https://help.ubuntu.com/community/HardyUpgrades
