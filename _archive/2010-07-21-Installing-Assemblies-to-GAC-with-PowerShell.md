---
layout: post
status: publish
title: 'Installing Assemblies to GAC with PowerShell'
slug: "Installing-Assemblies-to-GAC-with-PowerShell"
---
## I wrote a quick and dirty environment setup script in powershell to help other members of our dev team bootstrap their dev setups for a new project.

I just finished my first ever powershell script and I&#39;m pretty proud of it - even though [I did borrow a good portion of it][1] .

We're starting a pretty long-term project at work - it's scheduled for a final deliverable date set in July of 2011 - and with our small(er) dev team I'm pretty certain more hands than my own will be working on this over the course of the entire project.

This new project is going to have a few external dependencies (Moq, StructureMap, possibly MvcTurbine) and I wanted a quicker and friendlier way for a new developer coming into the project to get up-to-speed environment wise.

I originally started with a batch file. I know, I know. It&#39;s my comfort zone. Too many years of not having something like powershell at my finger-tips. After about 10 minutes of fighting with batch language limitations [I decided to move on and try my hand at using powershell][2]  to help automate the developer environment setup.

So I borrowed some code from Adam Weigerts blog to bypass the need for the gacutil (and the .Net Framework SDK, a whopping 100mb download!). Once I had this, I just had to come up with the code to recurse over all the .dll files that are stored in sub-folders within a "Source Dependencies" folder.

From concept to finish I think this took me about an hour to come up with, I'm no programming super soldier so anyone who is doing windows development should be able to throw together a powershell script to automate their common, repetitive and annoying tasks.

The code is included below, I&#39;ll be adding to it as the project goes on, pushing in some other environment setup features (dev db creation/sync would be the next feature for this). Now that I&#39;ve taken a swing at PowerShell I&#39;m really happy with it and I really am sorry it took so long for me to use it. If you&#39;re still automating things with Batch files, you&#39;re missing out. **Big.**
    
    BEGIN {
        Clear-Host
        $ErrorActionPreference = "Stop"
          
        if ( $null -eq ([AppDomain]::CurrentDomain.GetAssemblies() |? { $_.FullName -eq "System.EnterpriseServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" }) ) {
            [System.Reflection.Assembly]::Load("System.EnterpriseServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a") | Out-Null
        }
    
        $publish = New-Object System.EnterpriseServices.Internal.Publish
    }
          
    PROCESS {
        $files=get-childitem ".\Source Dependencies" *.dll -rec|where-object {!($_.psiscontainer)}
        foreach( $file in $files ) {
            $assembly = $file.fullname            
            if ( -not (Test-Path $assembly -type Leaf) ) {
                throw "The assembly '$assembly' does not exist."
            }
              
            if ( [System.Reflection.Assembly]::LoadFile( $assembly ).GetName().GetPublicKey().Length -eq 0 ) {
              throw "The assembly '$assembly' must be strongly signed."
            }
              
            Write-Output "Installing: $assembly"
              
            $publish.GacInstall( $assembly )
        }
    }
    
  [1]: http://weblogs.asp.net/adweigert/archive/2008/10/31/powershell-install-gac-gacutil-for-powershell.aspx
  [2]: http://twitter.com/codeimpossible/statuses/19095201324
