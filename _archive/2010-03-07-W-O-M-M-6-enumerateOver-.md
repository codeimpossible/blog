---
layout: post
status: publish
title: 'W.O.M.M. #6 - enumerateOver()'
slug: "W-O-M-M-6-enumerateOver-"
---
![works-on-my-machine-starburst][1] 

I've been focusing more and more on [ my port of Ling-to-Objects, Jsoq][2]  the past few weeks. It's still in really early stages and I'm not quite sure about it's actual usefulness but I'm learning a lot about JavaScript and having a ton of fun along the way!

Jsoq deals with arrays a lot. About 95% of it's use cases involve either looping through, altering, or creating arrays. Having a ton of `for` loops in my code just seems so... not right. For loops have always seemed dirty to me. They just aren't elegant enough.

Here is your normal, average, everyday `for` loop in Javascript.
    
    var array = "1,2,3,4,5,6,7,8,9,10".split(',');
    
    for(var i = -1, l = array.length; ++i &lt; l;) {
        alert(array[i]);
    }

This works. It's reliable and gets the message across. But what if we needed to loop over an array and get all the items that matched a condition? Using a `for` loop the code could look something like:
    
    var array = [ 1, 2, 3, 4, 5, 1, 2, 3, 4 ];
    var results = [];
    var condition = function(i) { return i%2 == 0; };
    
    for(var itt = -1, len = array.length; ++itt &lt; len;) {
        if( condition(array[itt]) ) {
            results.push(array[itt]);
        }
    }

This does work, I've written code like this many times before, and while *technically* there isn't anything wrong with it I think there is still room for improvement.

Jsoq is going to be handling arrays all over the place so the solution to this problem *needs* to be simple.

Here's what I need:

 - to loop over an entire collection and perform an action on each item.
 - If that action produces a result, the item is to be pushed into an array and returned to after the loop is done.
 - Also: it needs to be readable, someone else coming along should be able to determine what this thing is doing without too much difficulty. So I had my work cut out for me. 

A few hours later I had a decent function that I could use to replace a lot of the `for` loops I had. After some more refactoring I was able to wipe them all out and replace them with calls to `enumerateOver()`. Here is the latest version from source control:
    
    function enumerateOver(collection, work) {
        var result = [], val = [];
        
        if (isArray(collection)) {
            try {
                for (var i = -1, l = collection.length; ++i &lt; l;) {
                    result = work(collection[i], i);
                    if (typeof result !== "undefined" && result != null) {
                        val.push(result);
                    }
                }
            }catch (e) {
                if (e != jsoq.$break) throw(e);
            }
        
            if (val.length &gt; 0) {
                return val;
            }
            }else {
                try {
                    val = work(collection, 0);
                }
                catch (e) {
                    if (e != jsoq.$break) throw(e);
                }
                if (typeof val !== 'undefined') {
                    return val;
            }
        }
        return result == null ? [] : result;
    }

And here is the code to replace the `for` loops above, re-written to use `enumerateOver()`

    var results2 = enumerateOver(array, function(i, c) {
         return i%2 == 0;
    });

So by implmenting this function I was able to come up with a more readable, testable and streamlined codebase. Is this suitable for everyone? Definitely not, but I did it for a few reasons:

 1. Like I mentioned before. I've never been at ease with `for` loops and being able to replace them all with calls to a single function was a huge win for me
 2. The normal use-case didn't fit right. I needed to not only iterate over arrays but also return the results of work performed on those items. Doing this the "regular" way just wouldn't work (see previous reason)
 3. I thought this was a fun problem to solve

If you have any feedback, good, bad, or indifferent add a comment!

  [1]: http://codeimpossible.com/wp-content/uploads/2009/06/works-on-my-machine-starburst.jpg
  [2]: http://bitbucket.org/codeimpossible/jsoq
