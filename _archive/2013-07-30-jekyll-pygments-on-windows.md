---
layout: post
published: true
title: 'Albino, Pygments, Jekyll and "Bad File Descriptor" in Windows'
tags: "programming blogging github-pages jekyll"
---

_**Note:** this is posted here so I don't forget how to do this later. I don't take any credit for this fix, nor do I make any gurauntees about its quality/stability._

_**Another Note:** This fix is only relevant if you're getting the Bad File Descriptor error when your site is generated **and** you're running an older version of Jekyll (I'm running 0.11.2)._

I got a very strange error after enabling pygments in my jekyll-powered blog. My windows box kept outputting "Bad File Descriptor" instead of the source. One google search later I had located [a patch on a Ruby Gem named "Albino"](https://gist.github.com/madhur/1185645). Apparently, my older install of Jekyll uses Albino to call out to pygments (a python syntax highlighting library) to generate the syntax coloring markup.

Since Alibino is no longer maintained (deprecated for pygments.rb), I had to apply the patch manually to my albino source. I found the source file in `C:\Ruby193\lib\ruby\gems\1.9.1\gems\albino-1.3.3\lib\albino.rb`. Once I applied the changes everything ran perfectly. In case the gist goes away I've posted the diff below.

{% codeblock lang:diff %}
diff --git a/lib/albino.rb b/lib/albino.rb
index 387c8e9..b77d55e 100644
--- a/lib/albino.rb
+++ b/lib/albino.rb
@@ -1,4 +1,5 @@
 require 'posix-spawn'
+require 'rbconfig'

 ##
 # Wrapper for the Pygments command line tool, pygmentize.
@@ -84,11 +85,21 @@ class Albino
     proc_options[:timeout] = options.delete(:timeout) || self.class.timeout_threshold
     command = convert_options(options)
     command.unshift(bin)
-    Child.new(*(command + [proc_options.merge(:input => write_target)]))
+    if RbConfig::CONFIG['host_os'] =~ /(mingw|mswin)/
+      output = ''
+      IO.popen(command, mode='r+') do |p|
+        p.write @target
+        p.close_write
+        output = p.read.strip
+      end
+      output
+    else
+      Child.new(*(command + [proc_options.merge(:input => write_target)]))
+    end
   end

   def colorize(options = {})
-    out = execute(options).out
+    out = RbConfig::CONFIG['host_os'] =~ /(mingw|mswin)/ ? execute(options) : execute(options).out

     # markdown requires block elements on their own line
     out.sub!(%r{</pre></div>\Z}, "</pre>\n</div>")
{% endcodeblock %}

Thanks to [@madhur](https://github.com/madhur) for figuring this out and sharing it.
