---
layout: post
status: publish
title: 'See no eval(), hear no eval()'
slug: "See-no-eval-hear-no-eval-"
---

## Dynamic evaluation is a great way to shoot yourself in the foot.


Eval is a function in JavaScript that allows developers to invoke the js compiler and pass a string to be compiled and evaluated. Eval is often misused by developers which creates a [ticking timebomb in your codebase][1] . 


Here&#39;s [what Eric Lippert says on eval][2] :

&gt; 

Eric uses a pretty contrived example in his blog post, I don't think a lot of JavaScript devs are misusing eval in the way he shows in his post.


Most of the time, when I see a developer misusing eval, their real problem is their misunderstanding of how scoping works with JavaScript. Take the code below as an example.


I opened up the wayback machine and snagged some real production code from a previous job where we used JavaScript to scrape websites years before it was called "AJAX".


     var httpRequest = new ActiveXObject("Microsoft.XMLHTTP");
     var fStr = "var callbackFunction = function () {";
     fStr += "  if (nCrossings && 4==httpRequest.readyState) {";
     fStr += "    callback(httpRequest);";
     fStr += "  } else {";
     fStr += "  }";
     fStr += "}";

     //alert(fStr);
     eval (fStr);


* note: if this looks familiar it&#39;s because I had submitted part of this code to The Daily WTF waaaaaaaaay back in 2004. Check out [the original post here][3] .


Aside from the empty else (which is priceless), what are the other problems here? 


 1. it looks like shit. Seriously, this code makes my soul hurt.
 2. the eval statement is going to require a compilation to take place each time it's called. This can slow things down when it's used in a loop or used in a frequently called method.
 3. maintaining code like this is going to be a nightmare.
 4. debugging this is going to be... interesting. Note the commented out alert() statement.
 5. what if fStr were modified to include a variable that was assigned from user input? This could allow any number of injection attacks to take place on our codebase.

I know what the developer intended to do here. They wanted to pass the currently executing httpRequest object into the callback that is supposed to be called once the request recieves a response.

However, the developer didn't understand that httpRequest would be available to any function created at the same level as itself. This is because JavaScript, since it shares more in common with Scheme than Java, uses a function scope instead of a block scope.


    var test = 10;
    function testing() {
        alert(test);
        var test = 1;
        return function() {
            alert(test);
        }
    }
    var result = testing();
    result();


It will probably surprise you that the code above alerts "undefined" and then "1". This is a perfect example of function scoping in action. In JavaScript whenever you ask for a variable you'll be given the closest scoped variable with that name.


In this example, our first alert shows "undefined" because in the testing function the closest scoped variable named "test" has not been defined yet. Once we call the result() our variable has been defined, and a reference to it has been kept alive, so we see the alert "1".


Now that we know a bit more about scoping, how can we rewrite the code above to improve it?


All of that code above could easily be replaced with the following JavaScript:


    var httpRequest = new ActiveXObject("Microsoft.XMLHTTP");
    var callbackFn = function() {
        if( nCrossings && httpRequest.readyState === 4 ) { 
            callback(httpRequest); 
        }
    };
    
The only difference between this new code and the fStr mess above is that this doesn't take extra compilation time. This code will be easier to maintain, debug, read, and it doesn't open your codebase up to injection attacks.

99.9999% of the time, if you're using eval(), you're trying to cram a round peg in a toaster and are just hurting yourself in the long-run.


  [1]: http://www.youtube.com/watch?v=8vxEimC3HME#t=1m14s
  [2]: http://blogs.msdn.com/b/ericlippert/archive/2003/11/01/53329.aspx
  [3]: http://thedailywtf.com/Comments/JavaScript_eval().aspx
