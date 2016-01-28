---
layout: post
status: publish
title: 'This blog will be different tomorrow'
slug: "This-blog-will-be-different-tomorrow"
---

## I'll be blacking out this blog tomorrow in protest of SOPA/PIPA. Want to help?


In the words of [Minecraft][1]  creator [Notch][2] :

&gt; 

How can I join in?

It&#39;s easy - If you are a software developer you should fork [Sara Chipps's SOPA-PIPA protest git repository][3]  and add your website(s) to the ever-growing list of blackout participants.


Then, when the time comes (Midnight, EST on 01/18/2012) simply modify your website(s) to display the HTML in the index.html file that is located in the root of the repository.


Keep in mind that for this protest you should modify your website so that it returns a 503 response code header so that google and other search engines won't index your SOPA/PIPA protest page as original content.


There are instructions on the internet on how to do this in [Drupal][4] , [.htaccess][5] , [Php][6] , [C#][7] , and [Ruby on Rails][8] .


That made no sense

Well, if you&#39;re not a software developer we have a much easier option. Just include the javascript below on your website and visitors to your site will be sent to [http://protestsopa.org/][9] .


    &lt;script type="text/javascript"&gt;
        window.location.href = "http://protestsopa.org/";
    &lt;/script&gt;
    


I can't blackout my entire site but still want to help

Completely understandable, and there is an option for you as well! If you include the following javascript a 50px banner will be displayed on your site (example of the banner is included below the script):


    &lt;script type="text/javascript"
    src="https://raw.github.com/SaraJo/SOPA-PIPA-Protest-Page/master/javascript-only/banner.min.js"&gt;
    &lt;/script&gt;
    


[![][10] ][11] 


If you do decide to join in on the SOPA/PIPA blackout leave a comment here with your website urls and I'll make sure you get added to the list.


  [1]: http://www.minecraft.com
  [2]: http://notch.tumblr.com/
  [3]: https://github.com/SaraJo/SOPA-PIPA-Protest-Page
  [4]: http://www.kristen.org/content/503-http-status-code-when-site-down
  [5]: http://shiftcommathree.com/articles/make-your-rails-maintenance-page-respond-with-a-503
  [6]: http://php.net/manual/en/function.header.php
  [7]: http://stackoverflow.com/questions/4495961/how-to-send-a-status-code-500-in-asp-net-and-still-write-to-the-response
  [8]: http://stackoverflow.com/questions/8890351/return-a-specific-http-status-code-in-rails
  [9]: http://protestsopa.org/
  [10]: http://dl.dropbox.com/u/6291954/sopa_pipa_banner_example.PNG
  [11]: http://dl.dropbox.com/u/6291954/sopa_pipa_banner_example.PNG
