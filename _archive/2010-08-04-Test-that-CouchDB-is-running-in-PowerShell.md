---
layout: post
status: publish
title: 'Test that CouchDB is running in PowerShell'
slug: "Test-that-CouchDB-is-running-in-PowerShell"
---
I mentioned previously that I am working on [a dev environment setup script][1]  for a long-term project at work. The goal of this script is to get a developer's machine ready to work on the project, installing dependencies, creating dev/test databases, etc.

Since our project will be using [CouchDB][2]  my script should create/update a few databases when it is run (this part is coming soon!). But before that can happen the script needs to check for the presence of an active CouchDB instance.  

Since CouchDB has a completely HTTP-based API we can test for its existence by opening up our [favorite browser][3]  and visiting [http://localhost:5984][4] . This gives us:


    {"couchdb":"Welcome","version":"1.0.0"}


So my script is going to need to do an HTTP GET on the url above and compare it to the [JSON][5]  response that CouchDB should send back. Now normally, I might want this check to be less strict, maybe only checking that `{"couchdb":"Welcome"` was returned, but in this case I want the specific version number as well so this check will be fine going forward.

If I don't get the response I expect I'll want the script to fail out immediately; No sense in continuing with the environment setup if one of the key components is missing!
    
    $futonurl = "http://localhost:5984"
    $resp_couchdb = [string](new-object System.Net.WebClient).DownloadString($futonurl)
    
    if( $resp_couchdb.Trim() -eq "{""couchdb"":""Welcome"",""version"":""1.0.0""}" ) {
        Write-Output "CouchDB 1.0 Found!"
        # here we can continue setting up our test/dev document stores
    } else {
        throw("CouchDB v1.0 must be installed and running to continue...")
    }

Althought this script is pretty simple, it shows that PowerShell is not just MS-Batch 2.0. PowerShell lets me leverage .Net to do heavy lifting from the command-line that I wouldn't be able to do without creating a few console applications.
  [1]: http://codeimpossible.com/2010/07/21/installing-assemblies-to-gac-with-powershell/
  [2]: http://couchdb.apache.org/
  [3]: http://www.google.com/chrome
  [4]: http://localhost:5984
  [5]: http://en.wikipedia.org/wiki/Json
