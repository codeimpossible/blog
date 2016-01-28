---
layout: post
title: '1GAM Retrospective for January'
published: true
tags: "game-development 1gam"
description: My first 1GAM is Play-Z - a zombie survival game where you fight wave after wave of the undead!
---

_I finished my first 1GAM (one game a month) entry: [Play-Z](http://codeimpossible.com/play-z)! Play it and let me know what you think._

My first month is over and I'm pretty happy with the result. I've got my one game for this month, however I stutter-stepped a bit with another idea at the beginning.

I was asked to speak at a meetup to talk about making games in [ImpactJS](http://impactjs.com) early in the month so I decided to kill two birds and use the game I built as the topic of my meetup talk.

I dove in and ended up with ["Play-Z"](http://codeimpossible.com/play-z) a horde-mode survival game where you have to survive wave after wave of zombies. Play it and tell me what you think!

Also, it's [completely open-source too](https://github.com/codeimpossible/play-z)! Well, the code that I wrote is open source you'll still need a working copy of [ImpactJS](http://impactjs.com/) to get it running.

Now that we've got the pleasant stuff out of the way, let's cut to what I did right/wrong and what I've learned.

### Mechanics come first
I burned a lot of time at the start on art. It paid off, I think, but still that time could have been better spent working out the core gameplay mechanics - zombie health, character speeds, ammo, weapon damage, etc - at the start instead of doing it at the end.

In future games I'll start using really simple assets (maybe solid color blocks) to represent things in the game. You can see examples of this in other in-development games like [Strafe](https://www.youtube.com/watch?v=48GlqcTDqzc&feature=youtu.be&t=1m21s).

### A Sense of danger is important
Play-Z is a game where you try to survive wave after wave of zombies, getting points for each zombie you kill. The biggest problem for me initially was that there was no real sense of danger. The zombies were slow, and weapons were over-powered.

To fix this, I increased the reload times for the stronger weapons, increased the zombie speed and made each enemy pick their health when they were spawned from a range of 100%-150%. Also, I made the player move slightly faster than the enemies.

### Encourage the player to move around
Another thing that led to boring gameplay was that the player had no reason to venture out. They could sit where they spawned, laying waste to zombie after zombie with no real effort expended. Thats booooring! So I limited the ammo for the Shotgun and the Rifle and created some ammo crates that will spawn at areas on the map. Forcing the player to go out and leave their "comfort zone".

### Dodging is a good mechanic
Since the player can't move very fast, and enemies come from all sides, you're going to get into a spot where you're gonna die and it's not going to feel fun because you have no control over it. So I added dodging. When `[space]` is pressed, the player lunges past bad guys and comes up on the other side ready to fire. It's great.

### Art is fun, art and code is awesome
I'm absolutely in love with the low-fi 8-bit style in Play-Z. This feels like home, I was _cranking_ characters and animations out for this game. If I had more time with this game I would add a few more animations to the ["Big Boy"](https://github.com/codeimpossible/play-z/blob/master/media/big-boy.png) character and possibly add some other enemies (skeletons, or a werewolf maybe).

<div class="col-md-12">
  <div class="col-md-6">
    <img src="{{ site.url }}/assets/posts/1gam-jan/drips-slo.gif" style="height: 280px;" />
  </div>
  <div class="col-md-6">
    <img src="{{ site.url }}/assets/posts/1gam-jan/moar-splatter.gif" />
  </div>
</div>

<div class="clearfix">&nbsp;</div>

In the gifs above you can see the blood splatter code at work. When an enemy is hit with a projectile or when they die, they spawn [Giblets](https://github.com/codeimpossible/play-z/blob/master/lib/game/effects/zombie-giblet.js). Each giblet [determines randomly](https://github.com/codeimpossible/play-z/blob/master/lib/game/effects/zombie-giblet.js#L31-L34) if it can spawn a [SlimeDrip](https://github.com/codeimpossible/play-z/blob/master/lib/game/effects/slime-drip.js) - those pools of green you see in the top and bottom of the first gif - when it makes contact with a collision tile. [If the SlimeDrip is upside-down then it can spawn drips](https://github.com/codeimpossible/play-z/blob/master/lib/game/effects/slime-drip.js#L54-L59), which is determined randomly as well.

This dynamic blood splatter system worked out really well I'm pretty happy with it.

### Enemies need to react
Yo, Zombies are people too. At least they were... at some point. In The Walking Dead, when a character shoots a zombie it's a big deal. If the shot doesn't kill the zombie you have the immediate problem of said zombie ripping your face off.

So I added a feature that if you shoot a zombie in the back you are granted a 130% damage bonues but the zombie will come after you and they get a speed increase!

### Play, play, play your game
I think I split my time on Play-Z pretty evenly between development and play-testing. I'd play the game for 30-40 minutes each day, seeing how the controls felt and thinking of ways to make the gameplay faster, more exciting or to make it more difficult, etc.

The game I ended up with is way better than it would have been if I hadn't dedicated that much time to playing it. Theres room for improvement here though as I still feel like I didn't get the game as tuned as I would have liked.

### "If you put a button in a game, like a firing button, give someone a reason to not press it."
Jan Willem Nijman from Vlambeer game a talk in 2013: ["The art of screenshake"](https://www.youtube.com/watch?v=AJdEqssNZ-U). If you haven't watched this, you should stop reading right now and see it. Really.

In that talk he said the quote above, and I think I'll remember it forever. It really hit me hard, and I had this kind of "oh shit" moment. Where like, harps played and the skies opened up and I got to look into the secret game designer plane of existence...it was all stars and light and they should, have sent, a poet.

Anyway, Jan Willem makes a great point there. If you're not giving your player a reason to _not_ do something then your game is just a one-note song and your players are gonna have a bad time.

I added a lot of knockback to the stronger weapons in order to give you a moment of pause when you pull the trigger in close quarters.

Theres still probably a ton more to learn from these two games, but this is the list of stuff I had at the end here.

I'm pretty proud of the game I made in slightly less than a month but I'm definitely prouder of how many lessons I learned here that I can apply to future games.
