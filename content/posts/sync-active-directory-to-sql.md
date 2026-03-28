---
title: "Sync Active Directory to SQL"
date: 2011-01-17T22:29:00+0000
categories: ["SQL"]
tags: ["Synchronization", "Active Directory"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I have found that one of the most useful things to have sitting in a SQL database is User Information from Active Directory.  The information can then be displayed using a data view web part.  It's also useful for things like a custom content query web part that pulls information based on current user, or pull-down lists in InfoPath.  The options really are endless.  

## *Purpose

*
Our goal is to create a Timer Job that will take user information from Active Directory and put it into a system user table within SQL.  We will only be getting some of the user information available within AD.  You can download a couple of documents that have more information on different properties within AD.  I do not remember where I downloaded these documents, I did not create them, but they were both very useful on a couple of projects: 

[Get AD Documents](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)

The one piece of information that is a wee bit tricky to get is the user's last logon date and time (lastLogon).  The lastLogon property is stored on EACH domain controller for each user in AD, meaning that to get the correct last logon time we will have to get the information off each domain controller.  Another interesting piece of information is that the lastLogon value is the "number of 100 nanosecond intervals since January 1, 1601 (UTC)."

I am running two domain controllers, so the examples are for getting the information back from 2 domain controllers.

The first thing we need to accomplish is to create a Linked Server, Server Object called ADSI.

## *Create the ADSI

*
Active Directory Services Interface (ADSI) allows us to talk to Active Directory.[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjit2of5BuSFI3M6TkrvZB6mx_29SE3XcDYMZilqY93FRlO-c3iRhmKALgFLOE2hvcPW7sWPUu26ROYjRYPlDLbHftObwWJMsOWSJtcQxYt1dVELPpG1LMlIMMY6wTbwiZjzXYmKyC31_H-/s1600/Create+ADSI.jpg)

[Get ADSI SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)

The next step is to create a temporary table to store the information that has been pulled from Active directory.

## Create the Temp Table

## If you are going to bring back the lastLogon value, create a column for each of the Domain Controllers.
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjNqeBLRG9iAZqHoBva3dNIaYLLFXhLiUfpQ8neEO4JknVy_k_TYAU83dSAVIhdvjZENS5CqMFsQ6QXphfNDjdR8hhQD-nZfl_2KsWmBaRRrGD_JUvsTJeGQ4FuL3ICAMilMimE73YA3mL8/s640/Create+Temp+Table.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjNqeBLRG9iAZqHoBva3dNIaYLLFXhLiUfpQ8neEO4JknVy_k_TYAU83dSAVIhdvjZENS5CqMFsQ6QXphfNDjdR8hhQD-nZfl_2KsWmBaRRrGD_JUvsTJeGQ4FuL3ICAMilMimE73YA3mL8/s1600/Create+Temp+Table.jpg)[Get Temp Table SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)
## Create the System User Table
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRi6C3PMgb1qcUhcNwO14Yr2NQKYkhOzoC3Gh2eTKeL4zYvtjpSElz3hoRGjeIOKV9kjPmkuG1mxMxwXdGqq81XtefdpwCz5moHkQYH1G_nfIeLKJACEjHeWxlfPdF2-icPEgbUP3Y3Vi2/s640/Create+System+Users+Table.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRi6C3PMgb1qcUhcNwO14Yr2NQKYkhOzoC3Gh2eTKeL4zYvtjpSElz3hoRGjeIOKV9kjPmkuG1mxMxwXdGqq81XtefdpwCz5moHkQYH1G_nfIeLKJACEjHeWxlfPdF2-icPEgbUP3Y3Vi2/s1600/Create+System+Users+Table.jpg)[Get User Table SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)

[](http://yahoo.com/)Create a Scalar-valued Function

As far as I know, there is not an easier way to compare values within a single row than creating a function to compare values for us.  Once again, depending on how many domain controllers you have, will determine the number of columns to compare.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg1mLiz6rWAB7gQ9EA4EAK5hbZH8vPom6BCYykQ8_QKqm_6pm_0ZbRo5CgknFP4FFy31x3P-FSV4pcswPan1L7W5RStOcyhNnm6Tg5uB0x-qPY8JARbYi5ppVCaJSnfpnxVAWnrAS2IRu-P/s640/Create+Function.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg1mLiz6rWAB7gQ9EA4EAK5hbZH8vPom6BCYykQ8_QKqm_6pm_0ZbRo5CgknFP4FFy31x3P-FSV4pcswPan1L7W5RStOcyhNnm6Tg5uB0x-qPY8JARbYi5ppVCaJSnfpnxVAWnrAS2IRu-P/s1600/Create+Function.jpg)[Get Scalar-valued Function SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)
## Get the Information

Depending on how many Domain Controllers you have, you will have to adjust your scripts to match your tables.  Also, this script will only bring back 1,000 records at a time, due to a default paging setting in AD.  You can either get the admin to change the setting (not a good idea) or create an additional filter to bring back the users in chunks.  I would suggest filtering on the sAMAccountName, and bring back the users that have names that start with A-C, D-F,G-I,...  you get the idea.  So lets test...

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKihEIu7vjKG3TZT7v1BY6DaKf3wsvP9jsPupt7GioIt1AOePuGpZVNUjqmx3xnR104Fi48k0qQ4GmhKqjT9Pbqrr3IFYDlOtP8zzfH7bk7RKkWgOhVeh0pP621TUPbD-HFrnyKrPvpamm/s400/Test+Script.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjKihEIu7vjKG3TZT7v1BY6DaKf3wsvP9jsPupt7GioIt1AOePuGpZVNUjqmx3xnR104Fi48k0qQ4GmhKqjT9Pbqrr3IFYDlOtP8zzfH7bk7RKkWgOhVeh0pP621TUPbD-HFrnyKrPvpamm/s1600/Test+Script.jpg)Press F5 (Execute!)[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFYDGNus46koifbs7JW62QSz8i3W-3xibgrMptI2sj_YcdzvRZxbpRkgOd6REJNUYdkQs3pmunmgemQ-iDpIYYhyrMbVs3V-6oJB-JCKPNncFYARJmx33hH8RYDt-N7VHuWmOUiWGKR2df/s640/Test+Script+Results.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFYDGNus46koifbs7JW62QSz8i3W-3xibgrMptI2sj_YcdzvRZxbpRkgOd6REJNUYdkQs3pmunmgemQ-iDpIYYhyrMbVs3V-6oJB-JCKPNncFYARJmx33hH8RYDt-N7VHuWmOUiWGKR2df/s1600/Test+Script+Results.jpg)

[Get Test SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468) 

This script should bring back all "Person(s)" and "User(s)" within the CN from AD.  I have added a filter to remove anyone named 'Administrator' and 'Guest' as well.  I added the homePage for the fun of it for the test, and will not be importing the information within the Stored Procedure.

Once we have all the information being returned, we can go ahead and create a stored procedure so that the timer job will have something to run!

## Create the Stored Procedure

## I prefer to use the drop/create method before populating the Temp Table, but feel free to use an update command to modify the existing information. 
1)  Create a New Stored Procedure.2)  Go to your Temp_ADUsers table (right click) --> Script Table as --> DROP And CREATE To --> New Query EditorWindow.3) Copy Paste the script into the new stored procedure, remove the "Go" statements since you cannot use them in stored procedures.4) Add the 1st AD script to insert the Users into the Temp_ADUsers table.5) Add the 2nd AD script to insert the lastLogon from the second Domain Controller.5) Add the script to update the current users, the ones that are already in the database.6) Add the script that inserts new users into the database.

On the update, we have done a couple of things, such as change the userOrganization to a blank if NULL*, and drop the email address if their account has been deactivated in AD.  We do not want to send emails to people who are deactivated from the system...  And we have done the math to figure out the lastLogon time.

On the insert, we will set their ReadOnly column,  their System_Role, and do the math for lastLogon.

*[Get Stored Procedure SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)*
## *Create the Timer Job*
There are several ways to create a timer job, this is the way that I do it...

After we create the stored procedure, right click the procedure and select Script Stored Procedure as --> Execute To --> Agent Job...

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh5h1u8ZuhqGm7ig5Qu1-5WpVIYewen-9vhtoflakPM8o_LRTUNGU-stKeRW9s8J7qaYcc9OduoxsaovidyWUu3qB_5KR1t9I9TVTCpxqulDqyPP8jHuHi8CZjxKuLvdzF6NXXhl14NpZE-/s640/Create+Agent+Job.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh5h1u8ZuhqGm7ig5Qu1-5WpVIYewen-9vhtoflakPM8o_LRTUNGU-stKeRW9s8J7qaYcc9OduoxsaovidyWUu3qB_5KR1t9I9TVTCpxqulDqyPP8jHuHi8CZjxKuLvdzF6NXXhl14NpZE-/s1600/Create+Agent+Job.jpg)

Fill in your timer information...  NOTE:  This is only a temporary timer job...

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj0VQwztXcPszMKhaB1oufD5iyO-95EdZ7b6yrYtm-RoH_aG4pFlijBZ8r9QhSUURPAWgH-gFMmuJ6-Vxet8xThVRRUg7EU7nmVumujarBGhWAQR3474oNv7sa32-hPNmgvw6h_qgGOP65C/s640/Add+temp+timer+job.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj0VQwztXcPszMKhaB1oufD5iyO-95EdZ7b6yrYtm-RoH_aG4pFlijBZ8r9QhSUURPAWgH-gFMmuJ6-Vxet8xThVRRUg7EU7nmVumujarBGhWAQR3474oNv7sa32-hPNmgvw6h_qgGOP65C/s1600/Add+temp+timer+job.jpg)

Now we expand out SQL Server Agent, and Jobs, and look for our new timer job.  Open up the properties of our new timer job, go to the Schedules Page, and modify the schedule: 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiq0bPLgTvqHGFrUsEGJa7AMlpeHbH9JSx-X65r9TOGAHXa23HPkIAS-xAq0UDW8wBkY0mX9AwoDe4e0TR2v9hf_75sEVZkIh3llUa498-J_TAOBBCM2Ezla1kP4CPSHEtfxQMNXCh8H2nI/s640/Modify+Job+Schedule.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiq0bPLgTvqHGFrUsEGJa7AMlpeHbH9JSx-X65r9TOGAHXa23HPkIAS-xAq0UDW8wBkY0mX9AwoDe4e0TR2v9hf_75sEVZkIh3llUa498-J_TAOBBCM2Ezla1kP4CPSHEtfxQMNXCh8H2nI/s1600/Modify+Job+Schedule.jpg)

Right click your new timer job and select Start Job at Step...  and you should get a Success message or two!*[Get Timer Job SQL script here](http://cid-8e55aa8c038225f8.office.live.com/browse.aspx/AD%20Sync?wa=wsignin1.0&sa=514226468)*

*Conclusion*

You should now be able to synchronize Active Directory from 2 domain controllers into one SQL Table every 2 hours from 7am to 7pm.
