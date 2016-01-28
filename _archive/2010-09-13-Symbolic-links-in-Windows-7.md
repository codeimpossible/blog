---
layout: post
status: publish
title: 'Symbolic links in Windows 7'
slug: "Symbolic-links-in-Windows-7"
---
A symbolic link is a text file that contains a relative or absolute path to another directory or file. When an operating system receives a request for a path that resolves to a symbolic link the operating system loads the linked directory or file transparently.

So let's say I had the following symbolic link setup: 

`Folder A -linked-to-&gt; Folder B.`

When I browse to `Folder A` I would see the contents of `Folder B` as if they existed in `Folder A`! Since the folders are linked I can change any of the files in either location, and I can delete the symbolic folder (`Folder A`) without deleting the target folder!

Symbolic links have been around for a looooooong time ( in pretty much every other operating system ) but Windows got this feature only recently - as of Windows Vista mklink ships with the OS bits. 

To make a symbolic link, open a command line and use mklink (in Windows Vista and greater):
    
    mklink /D &lt;path_to_symbolic_dir&gt; &lt;path_to_target_dir&gt;
    
For example:
    
    mklink /D "C:\My Linked Folder" "C:\My Project Folder"
    


