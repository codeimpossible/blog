---
layout: post
status: publish
title: 'Works on my machine weekly project #1'
slug: "Works-on-my-machine-weekly-project-1"
---
[Download the source code mentioned in this blog post.][1] 

![works-on-my-machine-starburst][2] 

I'm a hopeless code junkie. I love to write code. Most people do one thing for work and then another for their hobby. My girlfriend for instance works as an IT / Systems Engineer  and her other thing is photography.

*My* other thing is writing **more** code. I never did this with any of my previous jobs (save dish washer... I did wash dishes when I was at home but I wasn't trying out new, cooler ways to wash them).

... So where was I? Oh, right code junkie. So, I really like to write code and what I've decided to do is start a new small project each week and try to use some new chunk of .Net or a new library and I'll post the end results of my efforts here for you all.

All of these projects will be offered under the CodeImpossible "works on my machine" code quality guarantee. But I'll never post something that flat-out doesn't work.

Sound good? Cool, let's kick it off - as the first entry into this space I'd like to present **TweetCommander**.

TweetCommander is a small .Net v3.5 console application that lies in wait, watching a twitter account for any new Direct Messages from another user (we'll call this person the "owner").

When a direct message from the owner is found, TweetCommander will check to see if it contains certain text, and depending on that text, will perform a series of actions on the machine it is running on.

TweetCommander will support three commands: "current_screenshot", "exit", and "set_interval".

 - Sending "current_screenshot" will tell TweetCommander to take a screen capture of the Windows desktop, upload it to TwitPic, and then send the url for that image to the owner user in a Direct Message.
 - Sending "exit" will cause TweetCommander to exit
 - The "set_interval" is followed by a number that represents the number of seconds TweetCommander should wait between requests for new direct messages from twitter. This is more to avoid the API limit than anything else.

**Settings**
TweetCommander will need to store the Twitter ID for the last successfully processed Direct Message somewhere so we aren't constantly processing the same commands over and over again. The end user won't need to be aware of this value but it's worth mentioning anyway.

We'll also need to store the wait interval so we don't lose this information if we need to restart TweetCommander for whatever reason.

Okay, so here is the settings file I have so far:

![tweet_mon_console_settings][3] 

**Working with Twitter**
All right so now we need to be able to interact with Twitter. Now, I don't want to write my own API library so I'll [go out and get the latest copy of TweetSharp][4]  which will give me a nice, readable interface to twitter's API. After getting this built I'll be able to get the most recent direct messages using the following code:
    
    var directMessages = FluentTwitter.CreateRequest()
                            .AuthenticateAs(
                                TWITTERACCOUNT_USERNAME,
                                TWITTERACCOUNT_PASSWORD)
                            .DirectMessages()
                                .Received()
                                .Since(Properties
                                        .Settings
                                        .Default
                                        .LastProcessedCommandID)
                            .AsJson()
                            .Request()
                            .AsDirectMessages();

Thats pretty freakin' sweet I must say. Tweet# really takes the brain work out of working with twitter and there is no way to look at that code and **not** understand what it is doing immediately. Epic win.

To get this running on your machine, just[ grab the source from my box.net folder][5] , and change these values at the top of the Program.cs file:
    
    // this is our "owner account" we will only act upon direct messages
    // send from this user
    private static string TWITTEROWNER  = "codeimpossible"; 
    
    // this is our listener accounts username
    private static string TWITTERACCOUNT_USERNAME = "someuser";
    
    // this is our listener accounts password
    private static string TWITTERACCOUNT_PASSWORD = "somepassword";

*Note: The solution file for this contains a reference to a compiled version of  the Tweet# library that contains *[*a quick patch I made for an issue that affects uploading an image to Twitpic*][6] *. However this issue has been fixed officially in the most recent source.*
  [1]: http://www.box.net/shared/hacsh3b3s4
  [2]: http://wpup.codeimpossible.com/2009/06/works-on-my-machine-starburst.jpg
  [3]: http://wpup.codeimpossible.com/2009/04/tweet_mon_console_settings.jpg
  [4]: http://code.google.com/p/tweetsharp
  [5]: http://www.box.net/shared/hacsh3b3s4
  [6]: http://code.google.com/p/tweetsharp/issues/detail?id=36
