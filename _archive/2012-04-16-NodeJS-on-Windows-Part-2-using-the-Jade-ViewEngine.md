---
layout: post
status: publish
title: 'NodeJS on Windows Part 2: using the Jade ViewEngine'
slug: "NodeJS-on-Windows-Part-2-using-the-Jade-ViewEngine"
---

## When we last left our nifty little NodeJS app it was spitting out "Hello World!" like a champ, let's see what other tricks we can teach it.


[Our node app that we created in part one][1]  is writing out our header "Hello World!" really well. But that&#39;s not really that cool. Plus, it doesn&#39;t scale. If we wanted to output more HTML we would find the current solution to be a giant pain in the ass.


It would be nice if we could use a templating engine to render HTML for us...


    $ npm install jade
    


[Jade][2]  is a templating engine like HAML, which you can use as well, but I chose Jade because it works really well with ExpressJS.


Let&#39;s create our first view. We&#39;ll create a folder named `views` in the root of our project directory and in there let&#39;s create a file named "index.jade" and paste the following markup in there:


    !!! 5
    html(lang='en')
      head
        title= title
      body
        h1= "Hello World!"
    


If you&#39;ve used HAML before then you should understand this markup, it creates a html5 document (!!! 5) and then sets the `title` property of our view model into the `&lt;title&gt;` tag, then just renders a simple `&lt;h1&gt;` in the body.


Easy enough. Now let&#39;s get our app to render this. Right now, our app has no idea that our views are in the `/views` directory, so let&#39;s clue it in. Paste the following code just before the `app.listen()` call:


    app.set('views', __dirname + '/views');
    


Lastly, let&#39;s replace the `app.get()` call with the code below:


    app.get(config.routes.index, function(req, res) {
      res.render('index.jade', {
        title: 'Hello World!',
        layout: false
      });
    });
    


This tells ExpressJS to render the jade view we created and we supply a simple viewmodel. Notice that we specify an extra property on the viewmodel: `layout`. This tells jade not to look for a layout template, a view that you can use to hold all your common markup, but instead render the view as it is.


now, save your file and run the app! You should see your new page which looks amazingly similar to the old page, but instead of using strings and rendering directly to the response stream, we've delegated the rendering task to a view engine.


This let's us keep all the view logic in a separate place so we only need to change our app code for app code related reasons. We can change the views for all the display requirements that we come up with.


So that wraps up this post. Next time I'll show you how to speed up your development process by eliminating a few steps from the save-stop-node-reload-node-refresh-the-page development cycle.


Also, I&#39;ve put all [the code for this and the rest of the series up on GitHub][3] ! Fork it if you like, if you see something odd send me a comment or a pull request!


  [1]: http://www.codeimpossible.com/2012/4/13/Getting-started-with-NodeJS-on-Windows
  [2]: http://jade-lang.com/
  [3]: https://github.com/codeimpossible/NodeJS-On-Windows
