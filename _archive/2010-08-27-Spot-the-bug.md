---
layout: post
status: publish
title: 'Spot the bug'
slug: "Spot-the-bug"
---
What is wrong with the code below?
 
Safe Assumptions:

 1. `_dictionary` is a valid non-null `Dictionary&lt;object,object&gt;`
 2. `_dictionary` contains items that will match the passed expression
 3. This code compiles with no warnings or errors
 4. This code **will** throw an exception at runtime.
    
    public IList&lt;TModel&gt; GetAllByCriteria&lt;TModel&gt; ( Expression&lt;Func&lt;TModel, bool&gt;&gt; criteria )
    {
        Func&lt;TModel, bool&gt; action = criteria.Compile();
        return _dictionary.Where( pair =&gt; 
            action( (TModel)pair.Value ) ).Cast&lt;TModel&gt;().ToList();
    }
    

If you don’t see it right away then you’re not alone. I spent a while debugging to catch this. If you do see it right away then pat yourself on the back.
