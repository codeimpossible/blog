---
layout: post
published: false
title: 'Indie Game Development with JavaScript'
slug: "indie-game-dev-javascript"
date: '2013-04-01 10:00 -04:00'
tags: "game-development idie gaming javascript impactjs pax"
---
## Or how we showed our game without killing totally ourselves

So as you've -probably- hopefully heard we were one of the winners of the Github Game-off with our entry [Rock Kickass](http://www.rockkickass.com). We only had a month to build the version that is playable at that website and I think it is freaking awesome.

After the contest we focused and banged out a lot of new weapons, enemies, bosses, enough content to justify us releasing a spin-off game for android! And all of this before our day at the indie mini-booth. We shipped the android game just 10 hours before we had to setup our booth. And that game didn't exist a week prior.

You heard that right. We ripped a portion of our game out, re-tooled the mechanics and UI and shipped it to android in less than a week. Whats even more impressive is that we both have day jobs. So this was strictly part-time work.

So how the hell did we pull this off? Glad you asked. We couldn't have done this without some major help from our toolset. You see it was all javascript. Yeah, our alpha at PAX was running in Chrome, on a semi-ok laptop. We built our game in javascript and ImpactJS which allowed us to deliver crazy features like killer saw-blades, wall climbing and 5 new levels all in the last day or so before we went to PAX.

We developed an entire Boss and AI in an afternoon. We built an infinite run version of our game in a day. ImpactJS allowed us to ship features that were baked enough for our user base to try out and for us to get feedback on at an insane pace.

For our contest entry we also added Google Analytics integration, specifically their events API so that we could track how players played the game. We got over 2600 unique players to the game after we won and the data was amazing. We could tell which level was the hardest for players to complete by watching the "death" events suddenly become more popular in the event flow than the "level load" events.

All of this was possible because our game ran on javascript, it was as easy as pasting a `<script />` tag into our page and adding calls to `_gaq.pushEvent()`. Boom, instant event tracking.


However, in the days before we stood at our booth, hell even in the moments leading up to the gates being opened, we were both questioning if Rock Kickass was really the next game we should work on. We have plenty of other ideas/options open to us, is Rock Kickass the thing that we should devote all of our focus to right now?

All of that changed when we started pitching the alpah to people. Our concept was simple and we had only solidified it a few days beforehand: Rock Kickass is Megaman meets Super Meat Boy. When we said this to one gamer as he stared at our little monitor showing the game his response was instant and powerful: "I LOVE IT!" he half-yelled as he sat down and started playing.

Collecting and analyzing data is the way that we grade how we are doing and what, if any, corrective actions we need to take are. The data from PAX was unanimous: Build Rock Kickass and get peoples hands on it.
