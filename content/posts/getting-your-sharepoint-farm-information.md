---
title: "Getting Your SharePoint Farm Information"
date: 2012-10-21T03:25:00+0000
categories: ["PowerShell", "SharePoint"]
tags: ["SharePoint 2010", "SharePoint 2013", "BackUp", "Documentation", "Upgrade"]
aliases:
  - "/2012/10/getting-your-sharepoint-farm.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

As we prepare our upgrade strategy for SharePoint Server 2013, one of the major requirements is documentation of our existing SharePoint 2010 environment.  This is also a good script to run when first getting your hands on an unknown farm.  This should save you a bunch of documentation time.

<Lecture>

In theory, you should be reviewing the output documentation of your environment on a weekly basis.  You could even go as far as importing the results into SQL, and tracking your growth and changes of your SharePoint Farm with SSRS.

</Lecture>

This script will create a folder, go through your farm, and create the appropriate .csv file as long as you have the appropriate permissions to run the script and you have permissions to access the data.

There are several blog posts on how to take all the .csv files and merge them into one .xlsx file.  Here are a couple that I found useful.  You will notice that they call the ComObject Excel.Application, so if you do not have Excel installed or the appropriate Office System Driver Connectivity Components installed, you will get an error.

This post by Jeff Hicks was very informational about how to get everything working:

[http://www.petri.co.il/export-to-excel-with-powershell.htm](http://www.petri.co.il/export-to-excel-with-powershell.htm)

This [page](http://gallery.technet.microsoft.com/office/7c56c444-2476-4625-b1d9-821f30280e44) actually has a .ps1 for you to download, but in the Q&A section, [imfrancisd](http://gallery.technet.microsoft.com/office/site/search?f%5B0%5D.Type=User&f%5B0%5D.Value=imfrancisd) has a script that works very nicely as well.

Update (01/10/2012)

Added -Limit All

Update (10/30/2014)

Added a check to see if Backup location ends with "\"

Added URL to Sandbox Solution location for output

Added site structure output for entire farm

Added Disposal of objects

Update (10/21/2015)

Moved Script to Gist

Fixed error in Get Features section thanks to Matthew Bramer ([@ionline247](https://twitter.com/ionline247))

Cleaned up Get Site Structure section to get ALL webs within each Site Collection by Web Application.
