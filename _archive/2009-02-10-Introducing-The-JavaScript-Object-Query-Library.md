---
layout: post
status: publish
title: 'Introducing The JavaScript Object Query Library'
slug: "Introducing-The-JavaScript-Object-Query-Library"
---

##

So, yeah I originally started writing this article back in september when I was about mid-way through JSOQ. Some things have changed since then and I've tried to keep the post up-to-date. Enjoy! Also, another similar library has come out recently: [JSINQ][1]  which is a really feature-rich LINQ to Object implementation in Javascript. Really bad-ass stuff.

The Elevator Pitch:

JSOQ is an open-source pet project of mine that lets you easily access the data that you need from large collections in JavaScript. It was developed over a two week period in September of 2008 and is currently at version 1.1.

Everyone probably just shrugged and said "so??" Well let me show you. 

Lets assume that you have the following JSON string from another web application like, say [twitter search results][2]  for example:

    {"results":[
    {
    "text":"Just another day at the office",
    "to_user_id":null,
    "from_user":"codeimpossible",
    "id":1194587890,
    "from_user_id":1316793,
    "iso_language_code":"en",
    "profile_image_url":"www.path.to.image.us",
    "created_at":"Tue, 10 Feb 2009 05:47:33 +0000"
    },
    {
    "text":"@spolsky thats easy! http:\/\/tinyurl.com\/2z42bs",
    "to_user_id":1357501,
    "to_user":"spolsky",
    "from_user":"codeimpossible",
    "id":1194582705,
    "from_user_id":1316793,
    "iso_language_code":"en",
    "profile_image_url":"www.path.to.image.us",
    "created_at":"Tue, 10 Feb 2009 05:44:51 +0000"
    }
    ]} 



In the sample above we only have two results but JSOQ has been tested to work with collections of tens of thousands of items.

The JSON object we have here is perfectly fine if we watned to show a whole bunch of data to the end user, but what if I only wanted to show the search results after a certain date? Or what if I only wanted the text from each message that was typed to this "spolsky" character?

To accomplish this in "straight" javascript I'd be looking at writing a lot of loops and if/else code and then I would still have all these extra properties that I don't even need.

JSOQ allows me to query a javascript object or collection of objects using a specified criteria and return only the properties/methods I want.

So lets go with the second option. Let's get all the .text members from all the messages that were sent to the "spolsky" user.

    var search_results = eval(our_json_result);
    var result = null;
    with(jsoq) {
        result = From(search_results.results)
                    .query('text')
                    .where(function(i) {
                        return i.to_user === "spolsky";
                    });
    }

And now our result object is filled with a bunch of other objects that would look something like:

    {
        text: "@spolsky thats easy! http:\/\/tinyurl.com\/2z42bs"
    }

Pretty simple eh? For more information on JSOQ please check back here in the near future or contribute to [the google code repository][3] .

  [1]: http://www.codeplex.com/jsinq
  [2]: http://search.twitter.com/search.json?q=codeimpossible
  [3]: http://code.google.com/p/jsoq/
