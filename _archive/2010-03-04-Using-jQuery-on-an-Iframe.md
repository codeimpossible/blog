---
layout: post
status: publish
title: 'Using jQuery on an Iframe'
slug: "using-jquery-on-an-iframe"
---
The other day I had to alter the stylesheets in a child IFrame when a user selected an item from a drop-down. My first draft was pretty ugly, it ivolved getting the DOM from the child IFrame (by getting it's [contentWindow][1]  or [contentDocument][2]  property) then getting the &lt;head&gt; of the DOM and looping over all the child items... yuck!

I coded up this jQuery extension method which will return a jQuery-wrapped DOM instance for any of the matched IFrames.
    
    (function($) {
        $.fn.extend({
            dom: function () {
                var $this = $(this);
                var getDom = function(o) {
                    if( !o || (!o.contentWindow && !o.contentDocument) ) {
                        return null;
                    }
    
                    var doc = (o.contentWindow || o.contentDocument);
    
                    return doc.document || doc;
                };
    
                var dom = getDom($this[0]);
    
                return dom === null ? $this : $(dom);
            }
        });
    })(jQuery);

So with this, getting the styles for a child IFrame is as easy as:
    
    $('iframe').dom().find('head link').each(function(index, item) {
        alert(item.href);
    });

*Unfortunately, this code will only work if your IFrame is hosting content that is on the same domain as its parent.*

  [1]: http://www-archive.mozilla.org/docs/dom/domref/dom_frame_ref5.html
  [2]: http://www-archive.mozilla.org/docs/dom/domref/dom_frame_ref4.html
