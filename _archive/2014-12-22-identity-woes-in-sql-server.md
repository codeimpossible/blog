---
layout: post
title: 'Identity woes in SqlServer'
date: 2014-12-22
published: true
tags: "sql programming troubleshooting devops sharepoint"
---

My wife was having some trouble with a sharepoint farm at her work - one of the sites had gone into a wierd semi-read-only mode. Any time someone tried to create or edit content - or upload a file, the site would show an error page and when she checked the error message the error message was less than helpful.

> Arithmetic overflow error converting IDENTITY to data type int.

We talked about it for a few minutes, we agreed that somewhere, in the deep dark bowels of the sharepoint database lurked a table whose identity column had hit the limit of it's datatype. Looking at the error message we knew it was an INT identity column, so the problem was going to keep happening until she could look at the table and clean it out.

There's just one problem. **We had no idea which table had the problem.** The stacktrace in the logs wasn't helpful, and the exception just showed that an error occurred, but not which table had the problem.

We tried attaching the sql performance monitor, thinking that when the error occurred it would tell us some more info. Big, fat, **nope.**

After more googling, we found the [IDENT_CURRENT](http://msdn.microsoft.com/en-us/library/ms175098.aspx) function that will return the last identity value generated for a specified table.

This was good, once we had a suspect table in "custody" we could run this and know for sure if it was causing the problem. But again, we weren't sure which table had the problem. More googling revealed two very awesome "hidden" stored procedures written by Microsoft.

## sp_MSforeachtable and sp_MSforeachdb
If you google for each of the stored procedures above, you won't find any MSDN docs on them. The only info I could find, aside from some stackoverflow posts, were two blog posts (here they are: [sp_MSforeachtable](http://weblogs.sqlteam.com/joew/archive/2007/10/23/60383.aspx), [sp_MSforeachdb](http://weblogs.sqlteam.com/joew/archive/2008/08/27/60700.aspx),) from 2007/2008. So, yeah, not much info out there on these.

Using `sp_MSforeachtable` made it insanely easy to run some sql for each table:

{% codeblock lang:sql %}
    EXECUTE testing..sp_MSforeachtable 'SELECT ''?'' as ''Table'', IDENT_CURRENT(''?'')+1 as ''Ident'''
{% endcodeblock %}

Now we could run this SQL and see which tables were at their identity limit! If you want to use the sql above make sure you change `testing` after `EXECUTE` to whatever your database name is.
