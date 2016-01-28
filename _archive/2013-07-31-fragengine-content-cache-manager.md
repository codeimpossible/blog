---
layout: post
published: false
title: 'FragEngine: Content Caching'
slug: "fragengine-content-cache-manager"
tags: "programming fragengine fragcastle game-development"
---

Loading textures XNA/MonoGame games can really be a pain in the ass. If you don't cache your assets you'll end up hitting the disk each time you load a texture, and allocating memory for each texture you instantiate.

FragEngine keeps all of this in check by enforcing the use of a content cache, cleverly named `ContentCacheManager`. When a FragEngine game starts, the ContentCacheManager will scan the /Content directory recursively, and load files as XNA/MonoGame Content Pipeline assets based on their parent directory:

 - `Sounds` each item in this directory will be loaded as a `SoundEffect`
 - `Songs` each items in this directory will be loaded as a `Song`
 - `Textures` each item in this directory will be loaded as a `Texture2D`
 - `Fonts` each item in this directory will be loaded as a `SpriteFont`

The `ContentCacheManager` stores each item in dictionaries broken out by asset type. The key for each dictionary entry is the normalized filename (the file name with spaces and special characters replaced) of the item.

Once the game has finished loading all of the content, you can get an instance of a loaded asset by calling the ContentCacheManager:

    var playerTexture = ContentCacheManager.GetTexture( "path/to/texture" );

You probably noticed that I didn't have to specify the normalized path when I asked for our texture. The content cache manager will normalize the given path for you before it attempts to look up the asset. This way you only have to remember the folder structure in your /Content folder and not our normalized dictionary keys.

### Not Done Yet
The Content Cache Manager is far from done, but so far it's helped us speed up the loading and use of assets in both Final Frontier and Rock Kickass. We're looking to do more work with Content Caching before we release 1.0 of FragEngine, including: adding the ability to configure your content directory location and group content together in categories and load/unload certain categories of content at different times.
