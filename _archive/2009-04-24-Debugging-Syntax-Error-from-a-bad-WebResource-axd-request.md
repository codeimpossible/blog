---
layout: post
status: publish
title: 'Debugging "Syntax Error" from a bad WebResource.axd request'
slug: "debugging-syntax-error-from-a-bad-webresource-axd-request"
---
"Syntax Error, Line: 2, Char: 0". How many of you out there have seen this error while working on a web project? Usually it's because of a forgotten semi-colon or parenthesis in some external javascript file. But sometimes it's something more sinister... Something darker, dirtier and just a little bit more evil. After seeing the error message, I opened up Internet Explorer's options dialog and unchecked the following options:

 - Disable script debugging (Internet Explorer)
 - Disable script debugging (Other)

![Internet Explorer Options Dialog][1] 


I then closed IE, returned to Visual Studio, stopped and re-started debugging (ctrl+shift+F5), and watched Solution Explorer as my page began to load.


![Solution Explorer Debugging Internet Explorer][2] 


Oh! That's not good. See the WebResource.axd request that has the same icon as the Default.aspx file? That means that a bad request was sent for an embedded resource and - most likely recieved a 404 page back instead of the javascript file, which caused our syntax error. Ok, so how do we figure out which WebResource reference caused the problem? Well, the only way that I have come up with so far, is to manually copy and paste each WebResource.axd url from the html source of the page to the address bar and navigate there. The pages that give return a file download are ok and the ones that don't will return a 404 page in the browser. After finishing this long process of elimination, I found the resource request that was causing my headache:


`/WebResource.axd?d=MaCiPhUUtdXNj16OOucV5e5lHCBZO...SNIP...`

So how do we figure out which resource has embedded this troublesome URL into our html source? I found the solution to that in [Irena Kennedy's blog post on "How to Decrypt an ASP.NET Encrypted Data"][3].

 1. Add a web page (e.g. DecryptData.aspx) to your web application. For the code to work, it must run in the same appdomain as the web application that created your encrypted string.
 2. Add a text box where you will type in the encrypted string.
 3. Add a label where you&rsquo;ll display decrypted results.
 4. Add a button.
 5. In code-behind on button click event, add the following code:
    
    System.Reflection.BindingFlags bf =
        System.Reflection.BindingFlags.NonPublic |
        System.Reflection.BindingFlags.Static;
    
    System.Reflection.MethodInfo DecryptString = 
        typeof(System.Web.UI.Page).GetMethod("DecryptString", bf);
    
    DecryptedData.Text = DecryptString.Invoke(
        null, 
        new object[] { EncryptedData.Text } ) as string;

After I created this page, I pasted the WebResource.axd URL (everything up to the &amp;t=) into the DecryptedData textbox on my DecryptData.aspx page, clicked the Decrypt button, and saw that one of my custom aspx controls was responsible. I then corrected the resource path and the page loaded as it should. See the screenshot below for an example of the DecryptData page, or [download the DecryptData .ASPX and Codebehind from my box.net folder][4] .

[![DecryptData page][5] ][6] 


  [1]: http://wpup.codeimpossible.com/2009/04/internet_explorer_options_2.jpg
  [2]: http://wpup.codeimpossible.com/2009/04/solution_explorer.png
  [3]: http://blogs.msdn.com/irenak/archive/2006/11/03/sysk-233-how-to-decrypt-an-asp-net-encrypted-data.aspx
  [4]: http://www.box.net/shared/uc9aea3999
  [5]: http://wpup.codeimpossible.com/2009/04/decryption_page.png
  [6]: http://wpup.codeimpossible.com/2009/04/decryption_page.png
