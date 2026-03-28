---
title: "Run a PowerShell v3 Script From a SQL Server Agent Job"
date: 2014-01-02T01:25:00+0000
categories: ["PowerShell", "SQL"]
tags: ["PowerShell v2", "PowerShell v3", "Timer Job", "SQL 2012", "SQL Job", "Task Scheduler"]
aliases:
  - "/2014/01/run-a-powershell-v3-script-from-a-sql.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

In a previous blog post, [*Synchronize Active Directory With SQL pt II*](/posts/synchronize-active-directory-with-sql/), I mention creating a Scheduled Task to run the PowerShell script to keep AD synchronized to SQL. Then a colleague of mine, [Don Kirkham](https://twitter.com/DonKirkham) (@DonKirkham) asked if it was possible to run a PowerShell script from within SQL. Well Don, the answer is "Yes", but there is a caveat... While SQL Server 2012 is running on Windows Server 2012, PowerShell for SQL is at version 2 and Windows Server 2012 is running PowerShell v3. We will address this issue later in the blog.

The other issue to be aware of is the warning from Microsoft, which says, "Each SQL Server Agent job step that runs PowerShell with the **sqlps** module launches a process which consumes approximately 20 MB of memory. Running large numbers of concurrent Windows PowerShell job steps can adversely impact performance."  So be careful not to choke your server.

Objective

Create a SQL Server Agent Job that runs a PowerShell script at a specific time and interval. In other words, a SQL job that mirrors the Scheduled Task created in my other blog post on how to* [Create a Scheduled Task With PowerShell](/posts/create-scheduled-task-with-powershell/)*.

PowerShell- SQL v2 vs Server v3 

As was just mentioned, PowerShell within SQL runs Version 2. If you were unaware that you could run PowerShell from within SQL Server Management Studio (SSMS), right-click on a folder, and select "Start PowerShell". Microsoft was nice enough to not make the option available for every folder, so for this blog, we are going to use the Jobs folder.

[![](/images/Start PowerShell.png)](/images/Start PowerShell.png)

This opens up a SQL Server PowerShell window and if you type: 

```
$PSVersionTable
```

you will see that SQL 2012 PowerShell is running V2.

[![](/images/PS v2 Table.png)](/images/PS v2 Table.png)

Here is where the problem begins. When you try to run the Sync AD to SQL script, it will error out, because the ActiveDirectory module is for PowerShell version 3, and when you try to import the module into version 2, your window will fill with RED.

[![](/images/PS v2 Error.png)](/images/PS v2 Error.png)

Instead of hacking things up and having different versions of the Sync AD to SQL script, we are simply going to try to open a PowerShell v3 window and execute the script within the new window. The first thing we want to try is to actually see if we can open a new instance of a PowerShell v3 window from within out SQL v2 window. So within the SQL Server PowerShell window type:

```
Start-Process PowerShell
```

and in the new window, type:

```
$PSVersionTable
```

[![](/images/PS v3 Table.png)](/images/PS v3 Table.png)

So now, all we need to do is tweak the script a bit to run the Sync AD to SQL script when the new v3 PowerShell window opens. The script that will be run is:

```
Start-Process PowerShell -ArgumentList "& 'C:\Scripts\SQL\Sync AD to SQL.ps1'"
```

Notice the single quotes around the location of the file that we are going to run.

Create The Accounts

Now that we have the proper syntax to get our v3 script run from within SQL, it is time to create the Job.

The nice thing about using PowerShell to create a Scheduled Task is the ease to create the RunAs account for the Scheduled Task. Within SQL, there are a couple of hoops that you will need to jump through to create the RunAs account for the Job.

The first thing that will need to be accomplished is to create a Credential to use as the RunAs account. Under the Security folder, right-click Credentials, and select New Credential...

[![](/images/New Credential.png)](/images/New Credential.png)

Within the New Credential window, fill out the boxes appropriately:

[![](/images/New Cred Window.png)](/images/New Cred Window.png)

or you can run a SQL Query to create it:

```
USE [master]
GO
CREATE CREDENTIAL [pcDemo\spAdmin] WITH IDENTITY = N'PCDEMO\spAdmin', SECRET = N'YourPassword'
GO

```

Next, we have to create a Proxy for the account to run a specific service. For this Job, we want the spAdmin account to run PowerShell, so we need to add that account as a Proxy. To accomplish this task, expand SQL Server Agent --> Proxies and right-click the PowerShell folder and select New Proxy...

[![](/images/New Proxy.png)](/images/New Proxy.png)

Within the New Proxy Account window, fill out the boxes appropriately:

[![](/images/New Proxy Account.png)](/images/New Proxy Account.png)

Or run a SQL Query to create the proxy:

```
USE [msdb]
GO
EXEC msdb.dbo.sp_add_proxy @proxy_name=N'PS spAdmin Proxy',@credential_name=N'pcDemo\spAdmin',@enabled=1
GO
EXEC msdb.dbo.sp_grant_proxy_to_subsystem @proxy_name=N'PS spAdmin Proxy', @subsystem_id=12
GO

```

Now that we have the Credential and the Proxy accounts created, the Job can be created.

Create The Job

From within SSMS connect to the SQL Server that is going to be running the Agent Job, and expand the object explorer to show the Jobs folder.

[![](/images/Object Explorer Jobs.png)](/images/Object Explorer Jobs.png)

Right-click the Jobs folder and select: New Job...

[![](/images/New Job.png)](/images/New Job.png)

This will open up the New Job window. Under the General page, you will want to give the Job a friendly name, and make sure that your Job is Enabled.

[![](/images/General Tab.png)](/images/General Tab.png)

From within the Steps page, we will create our one and only step for this post, which will be to execute the Sync AD to SQL.ps1 files in a new PowerShell v3 window using the spAdmin Account.

At the bottom of the window click the New... button, which will open up yet another window, the New Job Window. 

- Step name: Something user friendly and descriptive
- Type: PowerShell
- Run as: Select the proxy account you created earlier.
- Command: 
```
Start-Process PowerShell -ArgumentList "& 'C:\Scripts\SQL\Sync AD to SQL.ps1'"
```

[![](/images/New Job Step.png)](/images/New Job Step.png)

Click OK when finished 

The Steps page should now look like this:

[![](/images/Steps Tab.png)](/images/Steps Tab.png)

Next, click on the Schedules page, and click the New... button at the bottom of the Schedule List. This will open up a New Job Schedule window.

- Name: Add a user friendly name, such as Daily 2:00am Job
- Frequency: Change to Daily
- Daily Frequency

- Occurs once at: 2:00am

- Click OK when finished

Your Schedules page should look similar to this:

[![](/images/Schedules Page.png)](/images/Schedules Page.png)

Click OK for the new job when finished. You can also run the following SQL query to create the same Job:

```
USE [msdb]
GO
DECLARE @jobId BINARY(16)
EXEC msdb.dbo.sp_add_job @job_name=N'Sync AD to SQL Job',
     @enabled=1,
     @notify_level_eventlog=0,
     @notify_level_email=2,
     @notify_level_netsend=2,
     @notify_level_page=2,
     @delete_level=0,
     @category_name=N'[Uncategorized (Local)]',
     @owner_login_name=N'PCDEMO\spAdmin', @job_id = @jobId OUTPUT
select @jobId
GO
EXEC msdb.dbo.sp_add_jobserver @job_name=N'Sync AD to SQL Job', @server_name = N'SQL2012-03'
GO
USE [msdb]
GO
EXEC msdb.dbo.sp_add_jobstep @job_name=N'Sync AD to SQL Job', @step_name=N'Run Sync AD to SQL PowerShell Script',
     @step_id=1,
     @cmdexec_success_code=0,
     @on_success_action=1,
     @on_fail_action=2,
     @retry_attempts=0,
     @retry_interval=0,
     @os_run_priority=0, @subsystem=N'PowerShell',
     @command=N'Start-Process PowerShell -ArgumentList "& ''C:\Scripts\SQL\Sync AD to SQL.ps1''"',
     @database_name=N'master',
     @flags=0,
     @proxy_name=N'PS spAdmin Proxy'
GO
USE [msdb]
GO
EXEC msdb.dbo.sp_update_job @job_name=N'Sync AD to SQL Job',
     @enabled=1,
     @start_step_id=1,
     @notify_level_eventlog=0,
     @notify_level_email=2,
     @notify_level_netsend=2,
     @notify_level_page=2,
     @delete_level=0,
     @description=N'',
     @category_name=N'[Uncategorized (Local)]',
     @owner_login_name=N'PCDEMO\spAdmin',
     @notify_email_operator_name=N'',
     @notify_netsend_operator_name=N'',
     @notify_page_operator_name=N''
GO
USE [msdb]
GO
DECLARE @schedule_id int
EXEC msdb.dbo.sp_add_jobschedule @job_name=N'Sync AD to SQL Job', @name=N'Daily 2:00am Job',
     @enabled=1,
     @freq_type=4,
     @freq_interval=1,
     @freq_subday_type=1,
     @freq_subday_interval=0,
     @freq_relative_interval=0,
     @freq_recurrence_factor=1,
     @active_start_date=20140101,
     @active_end_date=99991231,
     @active_start_time=20000,
     @active_end_time=235959, @schedule_id = @schedule_id OUTPUT
select @schedule_id
GO

```

Testing

After Refreshing the Object Explorer, you should now see the Sync AD to SQL Job in the Jobs folder. Right-click the job and select Start Job at Step...

[![](/images/Test Job.png)](/images/Test Job.png)

Since there is only one step associated with the Job, so a new window will popup and your Job will start right away. Hopefully you will see a window that looks like this:

[![](/images/Job Test Success.png)](/images/Job Test Success.png)

If not, SQL have a very nice way to view the history of the Job. If you right-click the Job name, and select View History, the Log File Viewer window will popup with the Job History for the appropriate Job already selected. If you expand the failed job, you will receive insight into why your Job as failed:

[![](/images/Job Failed.png)](/images/Job Failed.png)

Conclusion

You should now be able to create a SQL Server Agent Job instead of a Scheduled Task to run a PowerShell script. Thank you again Don for the blog idea.
