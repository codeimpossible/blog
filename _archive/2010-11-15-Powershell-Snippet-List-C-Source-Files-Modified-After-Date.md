---
layout: post
status: publish
title: 'Powershell Snippet: List C# Source Files Modified After Date'
slug: "Powershell-Snippet-List-C-Source-Files-Modified-After-Date"
---

## An easy way to find what source files have been modified after a certain date in a project directory with powershell.
    
    $DateToCompare = Get-Date "8/17/2010 8:07 PM"
    
    Get-Childitem –recurse | 
        where-object {$_.lastwritetime –gt $DateToCompare} | 
        where-object {$_.extension -eq ".cs"}
    

