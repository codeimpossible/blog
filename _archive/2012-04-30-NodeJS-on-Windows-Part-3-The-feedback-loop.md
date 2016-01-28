---
layout: post
status: publish
title: 'NodeJS on Windows Part 3: The feedback loop'
slug: "NodeJS-on-Windows-Part-3-The-feedback-loop"
---

## Today we're going to learn how to use websockets to automate a huge portion of our development and testing process.


[In the last blog post][1]  I showed you how to install jade and create your first view. However the development cycle leaves a lot to be desired. Nothing sucks more than the web ux/ui development cycle. Make a change, restart the server (optional), refresh the browser, lather rinse, repeat.


I&#39;m going to show you something awesome. Open a powershell window ([you **are** using powershell aren&#39;t you?][2] ) and execute:


    $ npm install supervisor -g
    


This will install a package named Supervisor globally on your machine. Now, I could *tell* you what supervisor is but it&#39;s waaaaaay better to show you. It&#39;s ok, you can trust me, I&#39;m a blog post.... those never lie.


Anyway, let's run supervisor on the node app we created last time.


    $ supervisor app.js
    


You&#39;ll see the normal app bootup messages, when you visit `http://localhost:8000` the site should load normally and show you "Hello World!".


Aaaaaaand nothing is different.


What gives?

Yeah this doesn&#39;t sound awesome yet... but it will soon enough. Open your app.js and add the following to the `configure()` callback:


    console.log("Jared is super awesome!");
    


Now, save your js file and check your powershell window. Go ahead... I'll wait.


WASN'T THAT AWESOME?!?! Supervisor watches all your js files for changes, when changes are detected it will reload the app automatically! So let's change the header and see what happens.


    res.send("&lt;h1&gt;Jared is the best!&lt;/h1&gt;");
    


Hmmm, looks like supervisor reloaded but we have to reload the browser manually. That seems... tedious. What if we wrote some code to have the client listen for changes on the server and refresh the page as needed?


Kick it up a notch

First, we'll need a few more packages.


    $ npm install socket.io express
    


Socket.io is a framework that makes [websockets][3]  insanely easy. When we install it we pass the express argument so that socket.io installs with special bindings for ExpressJS. We&#39;ll use socket.io to send messages between the client (our browser) and the server.


We'll work on the server side first, changing our list of variables to include the socket.io object:


    var express     = require('express')
        app         = express.createServer(),
        jade        = require('jade'),
        io          = require('socket.io').listen(app),
        config      = {
                          routes:{
                              index: "/"
                          }
                      };
    


Oh! We'll also want to pass a date to our view, so let's add that to the viewmodel:


    res.render('index.jade', {
        title: 'Hello World!',
        layout: false,
        date: (new Date()).toString()
    });
    


I'll explain what this is for in a minute.


So we've got socket.io initialized, but we haven't told it to do anything except listen. This is fine for our purpose but normally you would have an event registered to tell socket.io what it should do with incoming requests but here, we really don't care.


When a file changes, supervisor will recycle the node server which will sever the connection with the client. We'll use this as the event on the client to trigger the page reload. Let's code that up now.


In our `index.jade` view, add the following markup:


    h2= date
    script(src="/socket.io/socket.io.js")
    script
      var socket = io.connect('http://localhost:8000');
      socket.on('disconnect', function (data) {
        window.location.reload();
      });
    


Here we have the socket.io client library loaded *(socket.io provides a route to ExpressJS that handles the `/socket.io/*` request)* and then we use that to open a long-polling connection to the server. When that connection gets disconnected (e.g. the server gets recycled when a file is changed) our page will reload.


Also, we now have our `date` property rendered out into an `&lt;h2&gt;` so we can tell when the page gets updated. All we have to do now is start supervisor and let it know we want it to watch more than just the .js files in our root:


    $  supervisor -w "views,app.js" -e "jade,js" app.js
    


The `-w` argument specifies files and directories that supervisor should watch for changes, and the `-e` argument specifies file extensions. So now when we change our view the server should recycle and the browser window should reload with the updated changes!


Using this method it would be very easy to link up css,less,sass files as well. Being able to do this with as little code as we have here is absolutely amazing and is one of the big reasons I'm getting more into NodeJS now.


Also, I&#39;ve put all [the code for this and the rest of the series up on GitHub][4] ! Fork it if you like, if you see something odd send me a comment or a pull request!


  [1]: http://codeimpossible.com/2012/4/16/NodeJS-on-Windows-Part-2-using-the-Jade-ViewEngine
  [2]: http://www.codeimpossible.com/2012/4/23/Reasons-to-use-PowerShell-instead-of-CMD
  [3]: http://en.wikipedia.org/wiki/Websockets
  [4]: https://github.com/codeimpossible/NodeJS-On-Windows
