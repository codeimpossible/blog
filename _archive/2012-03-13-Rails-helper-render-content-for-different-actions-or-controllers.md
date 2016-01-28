---
layout: post
status: publish
title: 'Rails helper: render content for different actions or controllers'
slug: "Rails-helper-render-content-for-different-actions-or-controllers"
---

## I needed to display different content based on which action/controller was executing.


On my blog homepage, I have a big block of text that does a nice little intro of me for visitors. This block of text is in a partial (the blog is written in Ruby, it&#39;s another one of my side projects: [Artigo][1] ) and is shown only when the index action is rendered.


I did this because I have to use the same layout for the entire theme (it's a limitation of the current blog's theming system), which means I had to come up with a way to only render content when a certain action or controller was "active". Here's how I did it:


      def is_active?(desc= {})
        controllerMatch = desc[:controller] == nil || params[:controller] == desc[:controller]
        actionMatch = desc[:action] == nil || params[:action] == desc[:action]
        idMatch = desc[:id] == nil || params[:id] == desc[:id]
    
        isactive = actionMatch && idMatch && controllerMatch
      end

So this can be used to hide/show content like so:

    &lt;% if is_active?( :controller =&gt; "posts", :action =&gt; "index") -%&gt;
      &lt;%= render :partial =&gt; "themes/codeimpossible/partials/callout" %&gt;
    &lt;% end -%&gt;

Let me know if this helped you!

  [1]: http://github.com/codeimpossible/Artigo
