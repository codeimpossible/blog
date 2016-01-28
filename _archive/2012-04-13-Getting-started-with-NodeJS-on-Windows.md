---
layout: post
status: publish
title: 'Getting started with NodeJS on Windows'
slug: "Getting-started-with-NodeJS-on-Windows"
---

## I got a chance to play around with nodejs earlier this week and it took me less than 5 minutes to get up and running


***Note: if you install express NPM will install the 3.x alpha release which is not compatible with the most recent version of connect. I've updated this post's instructions on installing the express module. Also, I published a package.json in [each of the examples on github][1] .***


If you work in the web development arena then you have to have heard of NodeJS. I mean if you haven't you should question the people you hang out with, because everyone seems to be talking about this.


I've wanted to play around with it for a while, I mean it's *server-side **JavaScript*** that is like everything I've ever wanted!


**Installation**

Installing NodeJS on windows used to be a royal pain in the ass. It involved installing Cygwin and compiling node from source, which now-a-days is like asking someone to build their car before they test drive it.


But now Windows is a first-class installation citizen! We have [our own nifty installer][2]  and it's pretty easy. Download, double-click, press install and let the nodejs goodnes wash over your computer.


**Packages**

Python has pip, Ruby has gems/bundler, .Net has Nuget, hell [everything has a package manager these days][3] , so it would stand to reason that NodeJS would have one. And NPM (Node Package Manager) is just that. Installing a package is pretty easy since NPM comes with the NodeJS install for Windows:


    $ npm install mongodb
    

BOOM, you've got the mongodb library for Node. All set. If you pull down a node app and want to make sure you have all the dependencies installed you just do this in the root of the app:


    $ npm install


This will read the `package.json` in the root of the app and download all of the dependencies that you are missing.


**Your first NodeJS App**

Creating a nodejs app is really simple, create new folder on your machine. Anywhere will do.


Now, let's add a package: (*currently v2.5.9 is the most recent version that works correctly)*:


    $ npm install express@2.5.9
    
ExpressJS is a web framework (like rails or django or asp.net) for Node. You can run `express` from a command line and it will create a whole project scaffold for you. We're not going to do that right now. We're just going to use express to handle routing for our server.


First thing we'll need to have is some variables. We need to load ExpressJS so we can set options and setup routes. Then we'll need to create our app, and then maybe a configuration variable to store settings.


    var express     = require('express')
        app         = express.createServer(),
        config      = {
                          routes:{
                              index: "/"
                          }
                      };
    

Okay, cool. Now we just need to tell the app what port to listen on. 8000 sounds good to me, how about you?


    app.listen(8000);
    

Then we simply register a few callbacks. The first one is executed when our app has finished starting. The second is run when a request comes in on a url that matches the route we pass in, which in this case is the root "/".


    app.configure( function() {
      console.log('Express server listening on port %d in %s mode', 
                      app.address().port, app.settings.env);
    });
    
    app.get(config.routes.index, function(req, res) {
        res.send("<h1>Hello World!</h1>")
    });
    


So when thats all put together, we end up with:


    var express     = require('express')
        app         = express.createServer(),
        config      = {
                          routes:{
                              index: "/"
                          }
                      };
    
    app.listen(8000);
    
    app.configure( function() {
      console.log('Express server listening on port %d in %s mode', 
                      app.address().port, app.settings.env);
    });
    
    app.get(config.routes.index, function(req, res) {
        res.send("<h1>Hello World!</h1>")
    });
    


Pretty simple right? Not a lot of code. Now, let's run it!


    $ node app.js
    


Then we just need to make a request to `http://localhost:8000` and we should see "Hello World!". And just like that you've got your first NodeJS app (albeit a very simple one) running on Windows!


Hang on to this code as we'll start modifying this app to add more features in the next blog post.


  [1]: https://github.com/codeimpossible/NodeJS-On-Windows
  [2]: http://nodejs.org/dist/v0.6.15/node-v0.6.15.msi
  [3]: http://codeimpossible.com/2011/11/29/Installing-the-Package-Control-plugin-for-Sublime-Text
