---
title: "Add SharePoint Snap-In to PowerShell ISE"
date: 2012-01-10T19:36:00+0000
categories: ["PowerShell"]
tags: ["SharePoint 2010", "SharePoint 2013", "ISE"]
aliases:
  - "/2012/01/add-sharepoint-snap-in-to-powershell.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Let me start off with saying that I, in no way, came up with this solution.  It was first shown to me by Shannon Bray ([http://shannonbray.wordpress.com](http://shannonbray.wordpress.com/)) when he and Gary Lapointe were maintaining spPowerShell.com.**
Since the site is no longer available, I have had to grab the following information from Kirk Evens ([Add Microsoft.SharePoint.PowerShell Snap-In to All PowerShell Windows](http://blogs.msdn.com/b/kaevans/archive/2011/11/14/add-microsoft-sharepoint-powershell-snap-in-to-all-powershell-windows.aspx)) and from Spence Harbar ([Adding SharePoint 2010 PoweShell cmdlets to your PowerShell ISE](http://www.harbar.net/archive/2010/05/03/adding-sharepoint-2010-poweshell-cmdlets-to-your-powershell-ise.aspx)).

Open up Windows PowerShell ISE (Run as Administrator), and run the following script:

```
# set the execution policy to run scripts
set-executionpolicy unrestricted -force

# Create the profile
if (!(test-path $profile.AllUsersAllHosts)) {new-item -type file -path $profile.AllUsersAllHosts –force}
psEdit $profile.AllUsersAllHosts

```

In the new Tab,

For SharePoint 2010** add the following:**

```
cd 'C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\14\CONFIG\POWERSHELL\Registration'
.\SharePoint.ps1
cd \ 

```

For SharePoint 2013** add the following:

```
cd 'C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\CONFIG\POWERSHELL\Registration'
.\SharePoint.ps1
cd \ 

```

Save the Profile tab (no, you do not run the script)

Again, thanks to Shannon, Kirk, Gary, and Spence for their posts!

UPDATE (01/16/2012)

After speaking with Shannon, he has finally moved over the blog...  you can find his ISE blog here...

[http://shannonbray.wordpress.com/2010/06/23/sharepoint-and-powershell-ise/](http://shannonbray.wordpress.com/2010/06/23/sharepoint-and-powershell-ise/)

UPDATE (10/21/2012)

Added the section for SharePoint 2013...
