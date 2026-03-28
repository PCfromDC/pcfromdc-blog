---
title: "Moving User SQL Databases Using PowerShell"
date: 2015-12-30T17:33:00+0000
categories: ["PowerShell", "SQL"]
tags: ["Database", "ACL"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I grew tired of manually moving databases around using a combination of SQL and "Copy / Paste" so wrote out a bit of PowerShell to save me some time and effort.

Notice that I am using the copy-item then deleting the object, not just moving the item. This is because of how permissions on objects are handled with copy vs move, plus I am paranoid about not having my original database handy if the move fails, or the moved database gets corrupted in transit.

#### 
Let's take a look at the code

In the first section we will be setting the variables.

The next step is to get the database information:

Once we have the database information, the database will need to be taken OFFLINE:

Once the database is offline, we can copy the file, set ACLs, and update the database with the new .mdf and .ldf file locations:

Then we can bring the DB back ONLINE

Once the DB is back ONLINE, wait for 10 seconds and delete the original database:

Here is a look at the code once it is all put together:

Updates

01/01/2016: Fixed issues with ACL for moved files by converting file location to UTC based path format.

01/02/2016: Updated to include snippets and comments

01/05/2016: Major update to fix $destination to UTC path, added copyItem function, item extension switch, updated outputs with write-output and write-verbose.
