---
layout: post
status: publish
title: 'Spot the bug - for/in and JavaScript'
slug: "Spot-the-bug-for-in-and-JavaScript"
---
I'm poking around in the JSOQ source this week and came across this gem. 
   
    var array = [
        { id: 1, num: 2 },
        { id: 2, num: 3}
    ];
    
    // find the item with num == 2
    for(var item in array) {
        if( item.num == 2 ) {
            alert(item.id);
        }
    }
    
The code above isn't calling `alert(item.id)`. Why is this?
