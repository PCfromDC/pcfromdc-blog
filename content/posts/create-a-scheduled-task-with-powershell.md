---
title: "Create a Scheduled Task With PowerShell"
date: 2013-11-29T15:30:00+0000
categories: ["PowerShell"]
tags: ["Windows Server 2012", "Task Scheduler", "OS"]
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

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgch-Gecew6xuNbZ7V7pL5qeONwIGrzwx1_ekjKQmSO94GeyJ-p0F6D1swftKT5Tr3YS4k89-9xIGywhT8UadIV2Z1fpMQbit8j1yqG_d1cknND3qm1z5tNznVMbW4KSUuITHXd5IfEieCS/s640/Task+Created.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgch-Gecew6xuNbZ7V7pL5qeONwIGrzwx1_ekjKQmSO94GeyJ-p0F6D1swftKT5Tr3YS4k89-9xIGywhT8UadIV2Z1fpMQbit8j1yqG_d1cknND3qm1z5tNznVMbW4KSUuITHXd5IfEieCS/s1600/Task+Created.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgGOIUiFoldGBHKq0AKaKT8Wi8tCn_4xZTBT5WrwKq-gadDrQjEtMamZ00bqbNUBydw8kgEm3OfYPbP22OS3PH2PKbIZrj6r5kgkz9rZv5rWl0jkdHrhbsj68dy8iCZQ6fBWgZJEx2sokn7/s640/General.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgGOIUiFoldGBHKq0AKaKT8Wi8tCn_4xZTBT5WrwKq-gadDrQjEtMamZ00bqbNUBydw8kgEm3OfYPbP22OS3PH2PKbIZrj6r5kgkz9rZv5rWl0jkdHrhbsj68dy8iCZQ6fBWgZJEx2sokn7/s1600/General.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi6BPRO3kPpj_-JKbjRureNxYqKIe69Tjv5XG-OVM0gl_DSES_P1B9dAg7jdzcGTh2G3AVB10Psxwe01WkgX95w4X1mJV41rtQBmoiUYZ_l5P5x8VfQIU0Nt6HOOaop-X2JjknI_Kpshc6O/s640/Triggers.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi6BPRO3kPpj_-JKbjRureNxYqKIe69Tjv5XG-OVM0gl_DSES_P1B9dAg7jdzcGTh2G3AVB10Psxwe01WkgX95w4X1mJV41rtQBmoiUYZ_l5P5x8VfQIU0Nt6HOOaop-X2JjknI_Kpshc6O/s1600/Triggers.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEijfM0tEIVNgA2UgOOJM_8ngNAWSY6az5UDQRgv9tMau93oWu8wlMz57SprcHmJNJnPTVJm9tKmopbKk9-iwVv0gn0e15z9XXP-MyZq81b53MB9jYQIcwy79daYr1EkLkCwL4jUtDYRtE_t/s640/Actions.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEijfM0tEIVNgA2UgOOJM_8ngNAWSY6az5UDQRgv9tMau93oWu8wlMz57SprcHmJNJnPTVJm9tKmopbKk9-iwVv0gn0e15z9XXP-MyZq81b53MB9jYQIcwy79daYr1EkLkCwL4jUtDYRtE_t/s1600/Actions.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhcV_bg6Z5XcRy6vw8LPQq_CeZmx_b31EZt__OssDI0sJk754oLH8NR5FMU87M7_rn0-_L67-Im4psvOhrxXLDsc84TJk6OkMfqP2bqh68CVqDx8QMmwprfhu1Q0PDwaUKjR8jlxbiZXl7I/s640/Conditions.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhcV_bg6Z5XcRy6vw8LPQq_CeZmx_b31EZt__OssDI0sJk754oLH8NR5FMU87M7_rn0-_L67-Im4psvOhrxXLDsc84TJk6OkMfqP2bqh68CVqDx8QMmwprfhu1Q0PDwaUKjR8jlxbiZXl7I/s1600/Conditions.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhIvZH3vvggBDh6kLrN-1SkQLkIrEzdZ5DWiNcf9uFXEDE3DOmT6D7FLtKxFzD7IQMJtmJv11w3DmtJonxhgVMz6LSIsxg8Rm5I0FB0Erb6xedKcB1_hW-gxa5ta25xWkk2h3Dyo_pL7mhyphenhyphen/s640/Settings.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhIvZH3vvggBDh6kLrN-1SkQLkIrEzdZ5DWiNcf9uFXEDE3DOmT6D7FLtKxFzD7IQMJtmJv11w3DmtJonxhgVMz6LSIsxg8Rm5I0FB0Erb6xedKcB1_hW-gxa5ta25xWkk2h3Dyo_pL7mhyphenhyphen/s1600/Settings.png)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbC-wB1THejNzlSI7MRjVrXh0sA2Ha7mMdpXqbQXU-2bALrHf8f1LytI-DA4052uJqoCUPrA3b92x8Q1IZtUCf1cHk6M6NlXrAxJT8UfY4KVCKV0RzAuo2XdbrhK5UKeIJS_xgkzSAMkbX/s640/History.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgbC-wB1THejNzlSI7MRjVrXh0sA2Ha7mMdpXqbQXU-2bALrHf8f1LytI-DA4052uJqoCUPrA3b92x8Q1IZtUCf1cHk6M6NlXrAxJT8UfY4KVCKV0RzAuo2XdbrhK5UKeIJS_xgkzSAMkbX/s1600/History.png)
