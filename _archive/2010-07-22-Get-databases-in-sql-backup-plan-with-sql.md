---
layout: post
status: publish
title: 'Get databases in sql backup plan with sql'
slug: "Get-databases-in-sql-backup-plan-with-sql"
---
I helped Tamara figure this out the other day and thought others might want to know. She needed to know which databases would be backed up when a given backup plan was executed. The server was a sql 2000 server and the machine she was using didn't have enterprise manager installed.

    SELECT 
        *
    FROM 
        msdb.dbo.sysdbmaintplans
    LEFT JOIN msdb.dbo.sysdbmaintplan_databases
    ON msdb.dbo.sysdbmaintplan_databases.plan_id = msdb.dbo.sysdbmaintplans.plan_id
    WHERE
        msdb.dbo.sysdbmaintplans.plan_name = 'PUT YOUR PLAN NAME HERE'


So if anyone out there finds themselves needing to know which databases will be backed up by a given backup plan, and for whatever reason you don't have Sql Management Studio or Enterprise Manager then this should work.
