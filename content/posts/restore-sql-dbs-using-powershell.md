---
title: "Restore SQL DBs Using PowerShell"
date: 2012-03-06T06:13:00+0000
categories: ["PowerShell", "SQL"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Of course the next logical thing after getting your backup script working is to create your restore script...  I used Donabel Santos's script from [http://www.sswug.org/articles/viewarticle.aspx?id=44909](http://www.sswug.org/articles/viewarticle.aspx?id=44909) as a reference.

```
$restoreDir = "c:\shared\Temp\" # last slash very important!

if ((test-path $restoreDir) -eq $false ) # Verify folder exists
  {
     $a = Read-Host("Path Not Found!")
     Exit -1 
  }

[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.SMO") | out-null
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.SmoExtended") | out-null
[Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.ConnectionInfo") | Out-Null
[Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.SmoEnum") | Out-Null

$files = get-childitem $restoreDir -recurse
foreach ($file in $files)
  {
    $backupFile = $restoreDir + $file
    $server = New-Object ("Microsoft.SqlServer.Management.Smo.Server") "(local)"
    $backupDevice = New-Object ("Microsoft.SqlServer.Management.Smo.BackupDeviceItem") ($backupFile, "File")
    $dbRestore = new-object("Microsoft.SqlServer.Management.Smo.Restore")
    $dbRestore.NoRecovery = $false;
    $dbRestore.ReplaceDatabase = $true;
    $dbRestore.Action = "Database"
    $dbRestore.Devices.Add($backupDevice)
    $dbRestoreDetails = $dbRestore.ReadBackupHeader($server)
    "Restoring Database: " + $dbRestoreDetails.Rows[0]["DatabaseName"]
    $dbRestore.Database = $dbRestoreDetails.Rows[0]["DatabaseName"]
    $dbRestore.SqlRestore($server)
  }

```
