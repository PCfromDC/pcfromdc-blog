---
title: "Create a Scheduled Task With PowerShell"
date: 2013-11-29T15:30:00+0000
categories: ["PowerShell"]
tags: ["Windows Server 2012", "Task Scheduler", "OS"]
aliases:
  - "/2013/11/create-a-scheduled-task-with-powershell.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Going into the GUI and creating a scheduled task is not rocket science, and now, either is creating a scheduled task via PowerShell. There is now a ScheduledTasks PowerShell module to help with automating this task. Below is a script for creating a Scheduled Task named *Sync AD to SQL*. The task runs daily at 6:00am and executes a PowerShell .ps1 file name *Sync AD to SQL.ps1*. This task is run as a specific domain user.

```
# Name of Task to create
$taskName = "Sync AD to SQL"
# Location of .PS1 file
$fileLocation = "C:\Scripts\SQL\Sync AD to SQL.ps1"
# UserName to run .PS1 file as
$user = "domain\userName"
# Password for above user
$password = "userPassword" 
# Create Task
$argument = "-Noninteractive -Noprofile -Command &'" + $fileLocation + "'"
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument $argument  
$trigger = New-ScheduledTaskTrigger -Daily -At 6am  
$settings = New-ScheduledTaskSettingsSet  
$inputObject = New-ScheduledTask -Action $action -Trigger $trigger -Settings $settings 
Register-ScheduledTask -TaskName $taskName -InputObject $inputObject -User $user -Password $password
```

If you are not Running the Task as a different user, comment out before the -User as seen below:

```
Register-ScheduledTask -TaskName $taskName -InputObject $inputObject # -User $user -Password $password
```

There are a bunch of properties that can be tweaked when creating a scheduled task. Below are the links to the appropriate technet articles for each cmdlet used:

[New-ScheduledTaskAction](http://technet.microsoft.com/en-us/library/jj649817.aspx)

[New-ScheduledTaskTrigger](http://technet.microsoft.com/en-us/library/jj649821.aspx)

[New-ScheduledTaskSettings](http://technet.microsoft.com/en-us/library/jj649824.aspx)

[New-ScheduledTask](http://technet.microsoft.com/en-us/library/jj649810.aspx)

[Register-ScheduledTask](http://technet.microsoft.com/en-us/library/jj649811.aspx)

### 
Results:

[![](/images/Task Created.png)](/images/Task Created.png)

[![](/images/General.png)](/images/General.png)

[![](/images/Triggers.png)](/images/Triggers.png)

[![](/images/Actions.png)](/images/Actions.png)

[![](/images/Conditions.png)](/images/Conditions.png)

[![](/images/Settings.png)](/images/Settings.png)

[![](/images/History.png)](/images/History.png)
