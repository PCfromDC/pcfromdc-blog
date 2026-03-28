---
title: "Backing Up SQL DBs Using PowerShell"
date: 2012-02-17T04:23:00+0000
categories: ["PowerShell", "SQL"]
tags: ["BackUp"]
aliases:
  - "/2012/02/backing-up-sql-dbs-using-powershell.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

My next project requires that I create several SQL Mirrors, and instead of backing up my databases with SQL, I thought I would try it in PowerShell.  The majority of the script is from Edwin Sarmiento's blog [http://www.mssqltips.com/sqlservertip/1862/backup-sql-server-databases-with-a-windows-powershell-script/](http://www.mssqltips.com/sqlservertip/1862/backup-sql-server-databases-with-a-windows-powershell-script/)  (Excellent Post)

```
$bkdir = "\\serverName\Shared\Temp" # Set Backup Path! (optional "C:\Temp")

if ((test-path $bkdir) -eq $false ) # Verify folder else create it...
  {
     [IO.Directory]::CreateDirectory($bkdir) 
  }

[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.SMO") | out-null
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.SmoExtended") | out-null
$s = new-object ("Microsoft.SqlServer.Management.Smo.Server") $instance

$dbs = $s.Databases
foreach ($db in $dbs) 
  {
     if(($db.Name -ne "tempdb") -and ($db.Name -ne "master") -and ($db.Name -ne "model") -and ($db.Name -ne "msdb")) 
          {
               $dbname = $db.Name
               $dbBackup = new-object ("Microsoft.SqlServer.Management.Smo.Backup")
               $dbBackup.Action = "Database"
               $dbBackup.Database = $dbname
               $dbBackup.Devices.AddDevice($bkdir + "\" + $dbname + ".bak", "File")
               $dbBackup.SqlBackup($s)
               write-host($db.name + " has been backed up.")
          }
  }

```
If you are saving to a network location, the SQL SA account and the person running the script need to have read/write permissions to the location.

Update: (03/05/2012)

Added save location verification else create folder.
