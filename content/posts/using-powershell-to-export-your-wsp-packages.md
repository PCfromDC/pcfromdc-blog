---
title: "Using PowerShell to Export Your .WSP Packages"
date: 2012-11-03T17:56:00+0000
categories: ["PowerShell", "SharePoint"]
tags: ["SharePoint 2010", "SharePoint 2013", "WSP Files", "Upgrade"]
aliases:
  - "/2012/11/using-powershell-to-export-your-wsp.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

During your upgrade from SharePoint 2010 to SharePoint Server 2013, we will need to make sure that we have all of the appropriate versions of our deployed WSP files.  If you have ever been a guest (consultant) on another farm, and your client has not been able to maintain a valid history of deployments, you could really run into a big upgrade problem.  Luckily for us, Shane Young, with help from Todd Klindt, wrote up a nice blog post on how to export and import your farm solutions using PowerShell called ***[Using PowerShell to export all solutions from your SharePoint 2010 farm and other fun](http://msmvps.com/blogs/shane/archive/2011/05/05/using-powershell-to-export-all-solutions-from-your-sharepoint-2010-farm-and-other-fun.aspx)***

I am only really worried about the extract portion of the blog, and have modified the script a bit to fit how I run my scripts.

```
#Get Backup Path
$bkdir = read-host("Enter Folder Location") # Get Backup Path!

# Set Backup Path if you want to hard code your path
#$bkdir = "\\serverName\Shared\Temp" (optional "C:\Temp")

# Verify folder exists
if ((test-path $bkdir) -eq $false ) # Verify folder else create it...
  {
     [IO.Directory]::CreateDirectory($bkdir) 
  }

# Add Snapin
Add-PSSnapin Microsoft.SharePoint.PowerShell -EA 0 

(Get-SPFarm).Solutions | ForEach-Object{$var = $bkdir + "\" + $_.Name; $_.SolutionFile.SaveAs($var)}

```

Thanks Shane for making life less stressful!
