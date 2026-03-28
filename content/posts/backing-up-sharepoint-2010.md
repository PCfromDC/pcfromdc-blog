---
title: "Backing Up SharePoint 2010"
date: 2011-09-29T03:33:00+0000
draft: true
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

**Farm Backup****
Backup-SPFarm -BackupMethod Full -Directory "\\server\share" -BackupThreads 5

Site Collection Backup****
Backup-SPSite -Identity "http://SharePointFix:101/" -Path "Path to Backup file" –UseSqlSnapShot

Important!**

- Your Central Admin app pool account must have read/write access to the location of the backups.
- Your SQL Service account must have read/write access to the location of the backups.
- If you're running a farm backup, the account you're running it as must have read/write access to the location of the backups
- The location must be accessible from the SharePoint machine the backup is running on.
- The location must be accessible from the SQL instance that SharePoint is trying to back up.
- This is why all the examples are UNCs, [\\server\share](), and not local paths, C:\backups

Save .ps1 file in c:\scripts\backup

run powershell -command c:\Scripts\backup\BackupSharePointFarm.ps1  in task scheduler

[http://social.technet.microsoft.com/Forums/en-US/sharepoint2010setup/thread/f755e7c1-bd0a-4c00-9be6-bbca83cf666b/](http://social.technet.microsoft.com/Forums/en-US/sharepoint2010setup/thread/f755e7c1-bd0a-4c00-9be6-bbca83cf666b/)
