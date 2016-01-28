---
layout: post
status: publish
title: 'MassiveRecord: A nice warm blanket for Massive'
slug: "MassiveRecord-A-nice-warm-blanket-for-Massive"
---

## I wanted to bring ActiveRecord over to .Net as a wrapper for the micro-ORM, Massive. I've taken some cool first steps and have something to show off!


I&#39;ve been writing code for almost 10 years now and one thing I&#39;ve learned is that working with Databases **sucks**. I was ecstatic when Rob Conery released [Massive][1]  last year. Massive makes databases fun again. You&#39;re not wondering what kind of SQL is going to be generated, it&#39;s fast and only about 600 lines of code.


If you haven't seen Massive in action here is a really quick snippet for you:


    var tbl = new Products();
    var products = tbl.All(where: "CategoryID = @0", args: 5);
    


Massive makes db work pretty simple but all the sugar that it pours on your db access layer isn't enough, I wanted more.


I started thinking how this could be made easier, and how it could be done Massive-style; One semi-large code file that you just drop into your project and then rock and roll.


So I came up with [MassiveRecord][2] . MassiveRecord is my attempt to move the fun stuff from Rails&#39;s [ActiveRecord][3]  over to the .Net world so developers can do things like this:


    var products = DynamicTable.Create("Products").FindByCategory("Televisions");
    


Behind the scenes, MassiveRecord uses the `DynamicObject` type in .Net 4.0 to give you some awesome convention-based shortcuts. I&rsquo;ll have some more posts that go into details about how this is done but for right now all you need to know is that it&rsquo;s not a lot of code and it&rsquo;s actually pretty easy to do.


So easy that if we wanted to get all fo the products in a category and with a certain price we just alter our method call a little:


    var products = DynamicTable.Create("Products").FindByCategoryAndPrice( "Televisions", 20.00 );
    


MassiveRecord. Just. Works. It takes convention-based method calls, converts them to Massive black-magic and gives you the result. And there is very little configuration needed. Put this in your main application initialization code (global.asax) and you&rsquo;re golden:


    DynamicTable.Configure( c =&gt; c.WhenAskedFor("Products").Use( s =&gt; {
        s.ConnectionString = "AdventureWorks";
        s.Table = "Products";
        s.PrimaryKey = "Id";
    }));
    


Now, instead of building out classes with custom methods you can keep all of your configuration in one place and rely on conventions to get things done with less code. I&rsquo;ve got more posts coming about MassiveRecord but for right now, [the code is up on github][4]  and you should download it, try it out and start submitting features and fixes!


  [1]: http://github.com/robconery/massive
  [2]: http://github.com/codeimpossible/massiverecord
  [3]: http://api.rubyonrails.org/classes/ActiveRecord/Base.html
  [4]: http://github.com/codeimpossible/MassiveRecord
