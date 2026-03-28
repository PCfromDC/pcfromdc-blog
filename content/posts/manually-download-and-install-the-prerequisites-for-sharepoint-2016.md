---
title: "Manually Download and Install the Prerequisites for SharePoint 2016"
date: 2015-08-24T23:40:00+0000
categories: ["PowerShell"]
tags: ["Prerequisites", "Off-line", "Installation", "Windows Server 2012R2", "Offline Installation", "Technical Preview", "Offline", "Windows Server 2016", "SharePoint 2016", "sp2016"]
aliases:
  - "/2015/08/manually-download-and-install-the.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

At some point within your career of deploying SharePoint, you will hopefully come across a scenario where your SharePoint servers are not allowed internet access. Most of the server farms that I work on are not allowed access to the Internet or are fire-walled/ruled out of the ability to surf the web or the ability to download items directly. This brings me to the need to be able to download the required files to a specific location and use that location for SharePoint's PrerequisiteInstaller.exe to complete its installation.

If you need to do an offline installation of SharePoint 2016, you will need to have the prerequisite files downloaded ahead of time. You will also need the SharePoint 2016 .iso ([download here](https://download.microsoft.com/download/0/2/C/02C2BDA8-63B1-4C88-B2F8-8D70D62AEE43/SharePoint%20Server%202016%20Beta%202%20English.iso)). These scripts are based off of the [scripts](https://gallery.technet.microsoft.com/office/DownloadInstall-SharePoint-e6df9eb8) provided by Craig Lussier ([@craiglussier](https://twitter.com/craiglussier)).

These scripts will work for SharePoint 2016 on Windows Server 2012R2 or on Windows Server 2016.

The only change that needs to be made is the location of the SharePoint prerequisiteinstaller.exe file in script #3 line #3. So, update the $sp2016Location variable before running. If the SP2016 .iso is mounted to the "D:\" drive, you have nothing to change and, in theory, this should just work for you out of the box.

The first thing that I like to do is to add the windows features:

The second step is to download the items required for the prerequisite installer:

The next step is to run the prerequisite installer. There is a requirement to restart the server during the installation and provisioning of settings:

The final step is to continue the installation of the prerequisites:

I hope that this saves you some time and headaches trying to get SP2016 installed and running correctly.

#### 
Updates

08/24/2015 Added ability to install on either Technical Preview for Server 2016 or Server 2012R2

08/25/2015 Added verbiage on updating $sp2016Location variable, and added workaround for PowerShell bug in download script.

11/25/2015 Updated for installation of SharePoint 2016 Beta 2 bits.

11/14/2017 Updated the scripts to install SharePoint 2016 on either Server 2012R2 or Server 2016.

11/29/2017 Updated the scripts to install SharePoint 2016 on Server 2016 Standard or Datacenter.

Thank you [Matthew Bramer](https://twitter.com/ionline247) for fixing this oversight.
