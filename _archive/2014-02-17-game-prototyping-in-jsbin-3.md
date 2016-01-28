---
layout: post
published: true
title: 'Using Jsbin to prototype game features: Part Three'
tags: "programming javascript fragcastle game-development"
---
Time for part three of my "prototyping games in jsbin" series. If you're not familiar with the other two posts ([part 1](http://codeimpossible.com/2014/02/02/game-prototyping-in-jsbin/), [part 2](http://codeimpossible.com/2014/02/09/game-prototyping-in-jsbin-2/)), you should read them otherwise this post won't really make much sense.

### First in, First out

Our 2d game engine is coming along nicely. We've got a way to create objects in the game world and all new Game Objects are drawn automatically. There is a slight problem however, controlling which objects appear on top of others is a bit difficult. By default the HTML5 canvas will draw objects bottom-up (also called first-in-first-out). This means that the first object added is at the bottom of the pile, and will be rendered first. This will cause problems later on because later on in your prototyping you'll want to render an object on top of all the others, but you won't have access to the `objects` array.

So, we need a way to control which objects are drawn on top of other; we need to be able to customize the draw order of the game objects while the game is running.

_In a 3d engine we'd accomplish this by adjusting the Z axis of each object, but our 2d engine only understands the X and Y axis so every object has the same Z axis and this cannot be changed._

First step is to create some objects to demonstrate the first-in-first-out rendering issue. Replace the `VerticalBox` and `HorizontalBox` code with the code below.

{% codeblock lang:javascript %}
    var YellowBox = Box.extend({
      color: 'yellow',
      height: 64,
      width: 64
    });

    var GreenBox = Box.extend({
      color: 'green',
      height: 64,
      width: 64
    });

    var RedBox = Box.extend({
      color: 'red',
      height: 64,
      width: 64
    });

    // after game.run()....

    game.objects.push( new RedBox( 70, 70) );
    game.objects.push( new YellowBox(100, 100) );
    game.objects.push( new GreenBox(130, 130) );
{% endcodeblock %}

This should draw the boxes stacked like the image on the left; try creating the `RedBox` last. Now the red box is rendered on top of both of the other boxes like the image on the right!

![RedBox, YellowBox, GreenBox](/assets/posts/game-proto-3/first-in-first-out-1.png)
![RedBox, YellowBox, GreenBox](/assets/posts/game-proto-3/first-in-first-out-2.png)


### Z-ordering: faking it till we make it

Since we don't have a Z-axis to manipulate, we'll have to fake it. Let's add a property to the `GameObject` class that we can use to track the draw order of objects. Change the `GameObject` constructor to match the code below.

{% codeblock lang:javascript %}
    Game.GameObject = function(x, y, options) {
      this.pos = this.pos || { x: x, y: y };

      this.depth = this.depth || options && options.depth || 0;
    };
{% endcodeblock %}

This code uses one of my favorite things about javascript: [truthy/falsey values](http://james.padolsey.com/javascript/truthy-falsey/) to reduce the ammount of code needed. We use the logical OR operator `||` as a shortcut; if `this.depth` is `undefined` or `null` or `0` then the `options.depth` value will be checked. If none of those values is truthy then the default value of `0` will be used.

This let's us specify a depth value three different ways:

{% codeblock lang:javascript %}

    // #1, as a property:
    var ObjectA = Game.GameObject.extend({
        depth: 1000
    });

    // #2, as a option passed to constructor
    // depth will be 100 even though its set to 1000 above!
    var game_object = new ObjectA( 100, 100, { depth: 100 });

    // #3, using the default
    // depth will be 0
    var game_object2 = new Game.GameObject( 0, 0 );

{% endcodeblock %}

So just having a depth property isn't going to make objects render in a different order. We've got to give the game engine the ability to re-order the objects. Let's add some code to do this is just before we call `.draw()` on each object, inside the games `draw()`.

{% codeblock lang:javascript %}

    // draw top down (lowest depth first)
    objects = objects.sort(function(a, b) {
      return a.depth - b.depth;
    });

{% endcodeblock %}

_Sorting the array each draw call isn't ideal, we'll refactor this later._

This code uses the built-in [`Array.sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) to re-order the array based on each objects `depth` property in ASCENDING order.

Being able to specify the depth on objects when we create them is going to be incredibly valuable during prototyping, think of enemies walking in front or in back of a wall, or the player passing behind something in the foreground.

Now, our code for spawning the first three boxes changes slightly:

{% codeblock lang:javascript %}

    game.objects.push( new RedBox( 70, 70, { depth: 100 }) );
    game.objects.push( new YellowBox(100, 100, { depth: 101 }) );
    game.objects.push( new GreenBox(130, 130, { depth: 102 }) );

{% endcodeblock %}

But we're back to having the RedBox in the background.

![RedBox, YellowBox, GreenBox](/assets/posts/game-proto-3/first-in-first-out-1.png)

Try changing the `depth` of the redbox to something like 105, or change the YellowBox depth to 1000. Having configurable depths in your game engine makes controlling z-index very easy.

### Spawning new objects

One more improvement we can make to this engine is to make it slightly easier to spawn game objects. Right now, we have to "new" up each game object and manually insert it into the array. Programmers are lazy, remember, we can't let this stand! Let's add a new method to our game object: `spawn`.

{% codeblock lang:javascript %}

    var spawn = this.spawn = function( Klass, x, y, options ) {
        var object = new klass(x,y, options);
        objects.push( object );
        return object;
    };

{% endcodeblock %}

Spawn() is a really nice shortcut. It will handle creating the object, passing the x and y coords and the options. And it will insert the new object into the objects array before returning it to us! This will let us add an object to the game and keep a reference to it for later use if we want. Also, this gives us a nice place to put our sorting code for the array. Remember how I said that sorting in each `draw()` was inefficient? Well, we'll change the code so that we only sort when a new object is added, since this happens much less frequently than `draw()` is called.

{% codeblock lang:javascript %}

    var spawn = this.spawn = function( Klass, x, y, options ) {
        var object = new Klass(x,y, options);
        objects.push( object );

        // sort objects to store top-down (lowest depth first)
        objects = objects.sort(function(a, b) {
          return a.depth - b.depth;
        });

        return object;
    };

{% endcodeblock %}

Now we can remove the sort from the game's `draw()`, update the code that creates the boxes, and things should still run pretty smoothly.

{% codeblock lang:javascript %}

    game.spawn( RedBox, 70, 70, { depth: 100 } );
    game.spawn( YellowBox, 100, 100, { depth: 101 } );
    game.spawn( GreenBox, 130, 130, { depth: 102 } );

{% endcodeblock %}

This wraps up part three, like the other examples, I've posted the [source up on Jsbin](http://jsbin.com/iPOzAJa/7/edit?js,output).

We started with nothing and now have a pretty decent - albeit extremely lite - game engine. Future work might include allowing a GameObject to have a texture or animation, or adding sound using the [HTML5 Audio API](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio).
