---
layout: post
status: publish
title: 'MassiveRecord: How DynamicObject gave C# its groove back'
slug: "MassiveRecord-How-DynamicObject-gave-C-its-groove-back"
---

## DynamicObject lets me create APIs in C# that would otherwise be impossible. It makes C# fun again.


I [blogged last time about MassiveRecord][1] , my attempt at porting ActiveRecord over to .Net as a wrapper for the micro-ORM Massive.


In this post I'm going to explain a bit about how MassiveRecord can have FindByID and FindByFirstNameAndLastName with less than 200 lines of code.


**First, a little about the dynamic keyword and DynamicObject.**


The CLR in .net has always been like a nosey neighbors. It wants to know when your garbage is collected, what objects you have laying around in your yard, and what type each of your variables is.


Okay, so I couldn't think of a good analogy for the last one, but I think you get the point.


In .Net 4.0 Microsoft added the dynamic keyword - mostly to make working with COM a lot easier - but the COM developers gains are ours too. The dynamic keyword gives us the ability to tell the CLR to back off.


It let' s us say "I know what this object is, and I don't want to care about its type." It's the developer equivalent of closing your blinds while your nosey neighbor - Carl Lee Richman (get it?) - sits, frustrated, in his living room with his binoculars.


DynamicObject is new to .Net 4 too. It's a sealed class which means you have to inherit from it but it gives you several overridable methods that are called in different situations when your inheriting object is dynamically dispatched. DynamicObject is the "man behind the curtain" for MassiveRecord. It's the thing that let's MR do it's awesome voodoo magic with the FindBy*() methods.


I won&#39;t go into all of the methods that DynamicObject provides but you should [read about them over at the MSDN website][2] .


**How does MassiveRecord do FindBy*()?**


Before I cover the FindBy*() methods, I think it would help to go over how MassiveRecord works from a high-level.


MassiveRecord is made up of two objects. The first is called DynamicTable which is a static class with a mini fluent interface that handles configuration and creation of the second class.


The second is the MassiveContextBase, this is the object that inherits from Massive's DynamicModel and allows users to do things like FindByUserNameAndEmail(). The code below shows how the inheritance looks:


    // from Massive.cs
    public class DynamicModel : DynamicObject {
    // ... snip ...
    }
    
    // from MassiveRecord.cs
    public class MassiveContextBase : DynamicModel {
    // ... snip ...
    }


Let's go a bit deeper into how DynamicObject is used in MassiveRecord.


Since Massive's DynamicModel inherits from DynamicObject, MassiveContextBase can override any of the methods on the DynamicObject class, which it does, specifically: TryInvokeMember().


    public override bool TryInvokeMember( System.Dynamic.InvokeMemberBinder binder, 
                                          object[] args, 
                                          out object result ) {
        if( binder.Name.ToLower().StartsWith( "findby" ) ) {
            var where = new StringBuilder();
            var method = binder.Name.ToLower().Replace( "findby", "" );
            var methodColumns = Regex.Split( method, "and" );
    
            for( int i = 0; i &lt; methodColumns.Length; i++ )
                where.AppendFormat( "{0} [{1}] = {2} ", 
                                        i &gt; 0 ? " and " : "", 
                                        methodColumns[ i ], 
                                        ToSql( args[ i ] ) );
            result = All( where: where.ToString() );
            return true;
        }
    
        var type = GetType();
        if( type.GetMethod( binder.Name ) != null ) {
            result = type.InvokeMember( binder.Name, 
                                        BindingFlags.Default | BindingFlags.InvokeMethod, 
                                        null, 
                                        this, 
                                        args );
            return true;
        }
    
        return base.TryInvokeMember( binder, args, out result );
    }


**The breakdown**


Don't worry about that gigantic block of code, I'll break it down and go over it piece by piece.


TryInvokeMember takes three arguments: a binder (information about what method the user asked for), the arguments the user passed (if any), and an out argument that should hold the return value.


Lets take a look at the first IF block:


    if( binder.Name.ToLower().StartsWith( "findby" ) ) {
        var where = new StringBuilder();
        var method = binder.Name.ToLower().Replace( "findby", "" );
        var methodColumns = Regex.Split( method, "and" );
    
        for( int i = 0; i &lt; methodColumns.Length; i++ )
            where.AppendFormat( "{0} [{1}] = {2} ", 
                                    i &gt; 0 ? " and " : "", 
                                    methodColumns[ i ], 
                                    ToSql( args[ i ] ) );
        result = All( where: where.ToString() );
        return true;
    }


First thing MassiveRecord needs is the method name. *Are you trying to invoke a MassiveRcord method or a Massive method?* This is done by checking for a "trigger phrase", specifically "findby". If MassiveRecord knows the method it is broken out into columns and each column is then put into a WHERE clause using custom code - ToSql() - to format the names.


Once this step is done MassiveRecord passes the WHERE clause off to Massive's All() method and returns the result to the user. TryInvokeMember demands we return a boolean value - true if the method was handled or false if the method was not. If we return a false here then the user would see an exception at run time saying that the method could not be found.


The next piece of code is something I'm actually pretty proud of.


    var type = GetType();
    if( type.GetMethod( binder.Name ) != null ) {
        result = type.InvokeMember( binder.Name, 
                                    BindingFlags.Default | BindingFlags.InvokeMethod, 
                                    null, 
                                    this, 
                                    args );
        return true;
    }


Since MassiveContextBase overrides TryInvokeMember every method call will have to go through TryInvokeMember() first. Even Massive-only methods like All() and Insert() will be popping through here on their way to Massive. This code checks to see if the method exists on the current class and if it does the method is executed.


The last piece shouldn't need any explanation. If both of the paths above failed, then pass the request onto Massive's implementation of TryInvokeMember and hope that it can sort out what needs to be done.


    return base.TryInvokeMember( binder, args, out result );


So that, in a nutshell, is how MassiveRecord does its voodoo to give you FindBy*() methods. I really like how DynamicObject lets me create classes and methods in C# that would otherwise be impossible. It makes C# fun again.


If you haven&#39;t done so already, please checkout [MassiveRecord on github][3] , fork it, submit patches, features, whatever!


 


  [1]: http://codeimpossible.com/2012/2/13/MassiveRecord-A-nice-warm-blanket-for-Massive
  [2]: http://msdn.microsoft.com/en-us/library/system.dynamic.dynamicobject.aspx
  [3]: https://github.com/codeimpossible/MassiveRecord
