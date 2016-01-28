---
layout: post
status: publish
title: 'Tracking .Net Service Requests in Fiddler'
slug: "Tracking-Net-Service-Requests-in-Fiddler"
---
Earlier today I had to debug a function in our code that was calling an external webservice at a clients' site. The webservice returns a list of items and the code on our end is supposed to place them in ascending order based on each items Order property.

The client pointed that the our order wasn't matching up with what they were seeing internally so I spoke with their developer who suggested that we make sure that the request wasn't being cached on my machine.

I opened up Fiddler locally and was surprised to see that the none of the request/response data for the connection to the webservice was showing up. 

Fiddler, for those that don't know is a really excellent Http tracing application that allows you to see what kind of http traffic is going in and out over your network connection. You can [download fiddler here][1] .

Thankfully, Fiddler runs a simple proxy service which you can point your service requests to in .net using:

    
    if (System.Environment.MachineName.ToLower().Equals("MyMachineName"))
    {
        service.Proxy = new System.Net.WebProxy("http://localhost:8888");
    }
    


After adding that code we were off and debugging our .Net service requests in Fiddler!
  [1]: http://www.fiddler2.com/fiddler2/
