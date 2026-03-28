---
title: "Backup and Restore SQL User Databases Using PowerShell"
date: 2016-01-29T02:31:00+0000
categories: ["PowerShell", "SQL"]
tags: ["SSMS", "Databases", "BackUp", "Database", "Restore"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

There are several ways how to backup and restore SQL databases. Over time, the way I back up and restore databases has changed.

Originally my backup database code looked like this:

The problem is that you needed to have SQL Server Management Studio (SSMS) installed for the code to run correctly. Having SSMS installed on a production server is not the best of ideas, so luckily PowerShell gives us the ability to backup all user databases very easily:

Now that we have our databases backed up, let's take a look at the old way that I use to restore databases:

Below is the newer, strictly using PowerShell way that I use to restore the just backed up databases:

Hopefully this will give you a couple of good solutions for backing up and restoring your SQL Server Databases through PowerShell.

-PC
