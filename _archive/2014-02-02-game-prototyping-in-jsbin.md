---
layout: post
published: true
title: 'Using Jsbin to prototype game features'
tags: "programming javascript fragcastle game-development"
---

If you're strapped for time prototyping a new game feature[;](http://www.youtube.com/watch?v=M94ii6MVilw) the last thing you want to do is waste time fighting your game engine/process/whatever. Here's how I built a simple game engine using HTML5 and JsBin to prototype game features quickly.

This last week I had to prototype a boss behavior for [Rock Kickass](http://rockkickass.com). It was something I'd never done before, going into it I wasn't really sure how I was going to get it done. Attempting to build the prototype in the actual game seemed daunting. I was worried that I'd spend more time messing with other things (rooms, scripts, objects, textures) instead of the prototype. I needed something bare-bones to work with. Something that had just enough code to get me to the next step and stay out of my way.

Which is why I decided to build a game engine in using HTML5's canvas on [JsBin](http://jsbin.com).

<span style="font-size: 22px">What?</span>

I know that sounds like a step backwards from the paragraphs above, but let me explain a bit.

### Jsbin + Canvas + Just Enough Js
I've had some (minor) experience building game engines before, so I felt confident that I could get a very (very) basic game engine that would use HTML5's canvas to draw elements. This would give me more control over how the code looked, and would increase the performance since most browsers will use the GPU to draw canvas elements.

It was important to me that the prototype be as close to our real codebase as possible - we're currently using GameMaker so JavaScript is a clear choice here. Also, it needed to be good on performance, I had done a prototype before that used HTML and DOM animations and it had be "ok" but it had a lot of performance issues.

First step is to get the foundation in place, we'll need a `<CANVAS />` element and some javascript code for the game object.

{% codeblock lang:html %}
    <!DOCTYPE html>
    <html>
        <body>
            <canvas id="canvas" height="480" width="640"></canvas>
        </body>
    </html>
{% endcodeblock %}

{% codeblock lang:javascript %}
    function Game() {
        var canvas = document.getElementById('canvas');
        var ctx = canvas.getContext('2d');

        var draw = this.draw = function() {

        };

        var update = this.update = function() {

        };

        var run = this.run = function() {
            update();
            draw();
        };
    }

    var game = new Game();
{% endcodeblock %}

Update will be used to handle movement and any other logic type operations. Draw... draws things to the screen. Run will be the main entry point for the game, it will run a set number of times a second (usually 60) and call `update()` and `draw()` each loop.rs-gameloop/).

Now, let's draw something. How about a yellow box? Change the draw method so it looks like the one below.

{% codeblock lang:javascript %}
    var draw = this.draw = function() {
        ctx.beginPath();
        ctx.fillStyle = 'yellow';
        ctx.rect(100, 100, 64, 64);
        ctx.fill();
        ctx.stroke();
    };
{% endcodeblock %}

Next, setup the `Game` object so it executes `run()` 60 times a second. This is something I would avoid doing in a real game engine but it's perfectly fine for our needs.

{% codeblock lang:javascript %}
    // insert after run()

    setInterval( run, 1000 / 60 );
{% endcodeblock %}

This will draw a yellow box on the screen!


### Adding movement

Making something move is just a matter of telling the game engine to change where it is being drawn for the new frame. Remember that a "frame" is one execution of both `update()` and `draw()`, or one call to `run()`.

in order to move our box, we're going to have to change a few lines of code. Replace `update()` and `draw()` with the code below.

{% codeblock lang:javascript %}
var box_pos = { x: 100, y: 100 };
var draw = this.draw = function() {
    ctx.beginPath();
    ctx.fillStyle = 'yellow';
    ctx.rect(box_pos.x, box_pos.y, 64, 64);
    ctx.fill();
    ctx.stroke();
};

var update = this.update = function() {
    box_pos.x += 1;
};
{% endcodeblock %}

Changing the `x` value of `box_pos` now moves the box to the right, but something odd is happening. The box is leaving this odd, ugly "snail trail" behind it.

![A square that is leaving a dark trail behind it. It looks ugly, terrifying but still a little familiar. Like you've met before. Maybe he was that guy you danced with on thursday when you told your friends you were tired but you went to that seedy dive bar in south boston to live a little. He never called.](/assets/posts/game-proto-1/snail.png)

This is happening because we're always drawing the box at it's new position, but we're never clearing the screen. This causes the new box to be drawn on top of the old box, but 1px to the right, which leaves the left border of the previous box still visible. This will be pretty easy to clean up. Change the draw method so it looks like the one below.

{% codeblock lang:javascript %}
var draw = this.draw = function() {
    // clear the canvas with a bg color
    ctx.fillStyle = "#ffffff";
    ctx.fillRect (0, 0, canvas.width, canvas.height);

    ctx.beginPath();
    ctx.fillStyle = 'yellow';
    ctx.rect(box_pos.x, box_pos.y, 64, 64);
    ctx.fill();
    ctx.stroke();
};
{% endcodeblock %}

Clearing the frame before we draw to it is a standard practice in game engines. There are other ways to handle this, XNA uses a two-frame draw method, showing one frame to the screen while drawing to another. It is constantly alternating between the frames to conserve memory and improve performance. Here, we're just drawing a large rectangle that is the same dimensions as our canvas with the same background color as our page.

Now your box should be moving rather nicely across the screen.

![A square that is moving innocently across the screen from the left to the right as all god-fearing, tax-paying squares do.](/assets/posts/game-proto-1/box-moving-1.gif)

[I've posted this up on jsbin](http://jsbin.com/iPOzAJa/4/edit) in case you have trouble getting this to work or if you just want to refer to the code in this post later.

That's enough for part 1, in part 2 I'll cover handling more than one box and using inheritance / objects to reduce the amount of code we need to write. I'll update this post when that part goes live.

Got any feedback, questions? Having trouble? Post a comment and I'll reply ASAP.
