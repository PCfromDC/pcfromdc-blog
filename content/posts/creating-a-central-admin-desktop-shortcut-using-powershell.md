---
title: "Creating a Central Admin Desktop Shortcut Using PowerShell"
date: 2011-07-04T17:21:00+0000
categories: ["PowerShell", "SharePoint"]
tags: ["SharePoint 2010", "Central Admin"]
aliases:
  - "/2011/07/creating-a-central-admin-desktop.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

While working on a deployment script, I thought it would be nice to add the Central Admin shortcut to the desktop of All Users.  You can get a lot of information from [http://ss64.com/vb/shortcut.html](http://ss64.com/vb/shortcut.html) concerning creating shortcuts, but if you want to add the shortcut for All Users:

```
# Add Central Admin Shortcut to All Desktops
$wshshell = New-Object -ComObject WScript.Shell
$desktop =  $wshShell.SpecialFolders.Item("AllUsersDesktop")
$lnk = $wshshell.CreateShortcut($desktop + "\SharePoint 2010 Central Administration.lnk")
$lnk.TargetPath = "C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\BIN\psconfigui.exe"
$lnk.Arguments = "-cmd showcentraladmin"
$lnk.Description = "Views the Central Administration Web Application."
$lnk.IconLocation = "%SystemRoot%\Installer\{90140000-1014-0000-1000-0000000FF1CE}\shcentadm.exe"
$lnk.Save()

```
