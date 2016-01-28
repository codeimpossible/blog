---
layout: post
published: false
title: 'FragEngine: Sprite Animations'
slug: "fragengine-sprite-animations"
tags: "programming fragengine fragcastle game-development"
---

<img src="http://content.screencast.com/users/codeimpossible/folders/Jing/media/adb6de56-ae02-491b-8b1f-27c1ed6c4a61/2013-07-23_1807.png" />

Creating Sprite-based animations in XNA is pretty much a huge pain in the ass. It involves writing a lot of code and usually you'll end up creating a class that you can re-use in your other games.

John and I wanted creating animations in FragEngine to be stupid easy.

    public class Hero : Entity {
        public override void Initialize()  {
            Animations = new AnimationSheet( @"Textures\player", 64, 64 );

            Animations.Add( "idle", 1f, true, 0, 1 );
            Animations.Add( "run", 0.07f, true, 12, 13, 14, 15, 16, 17, 18, 19, 20 );
            Animations.Add( "jump", 0.09f, false, 21, 22 );
            Animations.Add( "wallSlide", 1f, true, 2 );
        }
    }


Here is a really basic `Hero` game object that inherits from `Entity` which is the base of all game objects in FragEngine. Every `Entity` can have one or more Animations, which you specify in the `Initialize()` like we're doing above. The animation needs three pieces of information: The path to the texture in the [Content Cache](/2013/07/26/fragengine-content-cache-manager/), and the height and width of each frame in the animation.

Animations expect their texture to be a sprite sheet where frames are laid out in a grid format. In the future we might add support for Texture Packer sprite sheets but for now grid format is the only layout supported.

After the animation is created you can add new animations by calling `Add( animName, duration, repeat, frames[] )`.

 - `animName` is the name of the animation, this is used to change the current animation
 - `duration` this is the total run-time of the animation being specified
 - `repeat` should this animation loop? if only one frame is passed then the animation will not loop, but loopCount will increment as if it is
 - `frames[]` is a params array of integers that contains the zero-based index of the frame(s) for the animation

Setting animations is just as easy:

    CurrentAnimation = "run";
    //
    // or...
    //
    Animations.SetCurrentAnimation("run");
