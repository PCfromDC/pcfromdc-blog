---
title: "Upgrade to Server 8 (beta) With SharePoint 2010"
date: 2012-03-18T23:29:00+0000
tags: ["SharePoint 2010", "Windows Server 8", "Upgrade", "Server 8"]
aliases:
  - "/2012/03/upgrade-to-server-8-beta-with.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

With the unveiling of the new Server 8 beta OS, and SQL 2012, I decided to see how I can upgrade my development farm. 

Host:

Server 2008R2, 6 cores, 16 GB RAM.

Farm: All running Server 2008R2

OU-01: AD/DNS

SP-01: SharePoint 2010 SP1, August CU

SQL-01: SQL2008R2

I started by upgrading my Host OS via USB.  This was not as much fun as I was hoping, but in the process, I did find a great utility for formatting USB drives made by HP ([http://download.cnet.com/HP-USB-Disk-Storage-Format-Tool/3000-2094_4-10974082.html](http://download.cnet.com/HP-USB-Disk-Storage-Format-Tool/3000-2094_4-10974082.html)).  So you will need to download the ISO to USB tool from Microsoft called the [Windows 7 USB/DVD](http://www.microsoftstore.com/store/msstore/html/pbPage.Help_Win7_usbdvd_dwnTool), you can download the tool [here..](http://images2.store.microsoft.com/prod/clustera/framework/w7udt/1.0/en-us/Windows7-USB-DVD-tool.exe).  Run the program and follow the directions to create the boot-able USB.

So with the Host OS upgraded and my Snapshots taken, it was time to tackle my SP-01 box.  I did not detach from the farm, or do anything special, I just attached the ISO and ran E:\Setup.exe from the DVD.

      
[![](/images/1-Install Now.jpg)](/images/1-Install Now.jpg)

[![](/images/2- Get Updates.jpg)](/images/2- Get Updates.jpg)

[![](/images/3- Use GIU.jpg)](/images/3- Use GIU.jpg)

[![](/images/4- Accept EULA.jpg)](/images/4- Accept EULA.jpg)

[![](/images/5- Select Upgrade Path.jpg)](/images/5- Select Upgrade Path.jpg)

[![](/images/6- Compatibility Report.jpg)](/images/6- Compatibility Report.jpg)

[![](/images/7- Upgrade Setup Window.jpg)](/images/7- Upgrade Setup Window.jpg)

[![](/images/8- After a bit of time.jpg)](/images/8- After a bit of time.jpg)

[![](/images/9- Thats a good sign.jpg)](/images/9- Thats a good sign.jpg)

With the installation completed, we are going to be looking at my 3 web applications,  a Classic Authentication Application., a Claims based Authentication, and a Mysites Web Application, which is also a Classic Authentication Application type.

[![](/images/Web Applications.jpg)](/images/Web Applications.jpg)
Now that we have the OS installed, and without even logging into the server, let's see what our sites look like.

Here is our Classic Authentication Site...

[![](/images/10- Classic Site.jpg)](/images/10- Classic Site.jpg)

Here is our Claims Site (not a good sign)...

[![](/images/11- Claims Site.jpg)](/images/11- Claims Site.jpg)

Here is our My Site... 

 
[![](/images/12- Mysite login.jpg)](/images/12- Mysite login.jpg)

This is a good sign!

[![](/images/13- Mysite error.jpg)](/images/13- Mysite error.jpg)

This... not so much...

At this point I figured it is a Claims vs Classic authentication issue and a User Profile Issue.  Let's look at CA and make sure that everything is running.

Logging into my Windows Server 8 box, I notice a lot of errors right off the bat...

[![](/images/16 Server Manager.jpg)](/images/16 Server Manager.jpg)

So open up the event viewer to see what issues I am truly having...

[![](/images/17- WebHost Error.jpg)](/images/17- WebHost Error.jpg)

[![](/images/18- Claims Error.jpg)](/images/18- Claims Error.jpg)

Since all of our errors seem to stem from the STS having issues, lets take a look at our services and make sure that everything is up and running.

 
[![](/images/19- Services on Server.jpg)](/images/19- Services on Server.jpg)

Service on Server

[![](/images/20- Service Applications.jpg)](/images/20- Service Applications.jpg)

Web Applications

At this point, I decided that the STS will need to be provisioned again.  But how to run PowerShell in Server 8?

Server 8 PowerShell (.Net 4.0) will not run the PowerShell v2.0 cmdlets.  You will need to install a compiled PowerShell application like PowerGUI to run PowerShell v2.0 (.Net 3.5).  Read step #6 in Craig Lussier blog post [Install SharePoint 2010 on Windows Server 8 Beta](http://craiglussier.com/2012/03/01/install-sharepoint-2010-on-windows-server-8-beta/).  You will want to download [PowerGUI.3.1.0.2058.msi](http://powergui.org/servlet/KbServlet/download/3732-102-5885/PowerGUI.3.1.0.2058.msi) to run your PowerShell commands.  After downloading and installing, it will open a session of PowerGUI.  Do not upgrade, and close the program.  You will want to run PowerGUI as Administrator.

run the script...

```
Add-PSSnapin Microsoft.SharePoint.Powershell -EA 0
$sts = Get-SPServiceApplication | ?{$_ -match "Security"}
Write-Host($sts)
Write-Host($sts.Status)
$sts.provision()

```

lets verify sites...

Here is our Classic Authentication Site (still working)...

[![](/images/21- Verify Classic.jpg)](/images/21- Verify Classic.jpg)

Here is our Claims Authentication Site... 

[![](/images/22- Verify Claims.jpg)](/images/22- Verify Claims.jpg)

Here is our My Site...

[![](/images/23- My Site.jpg)](/images/23- My Site.jpg)

[![](/images/24- My Profile.jpg)](/images/24- My Profile.jpg)
