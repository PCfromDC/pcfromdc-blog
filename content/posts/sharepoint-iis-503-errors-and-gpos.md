---
title: "SharePoint, IIS, 503 errors and GPOs"
date: 2013-07-16T03:22:00+0000
categories: ["SharePoint", "SQL"]
tags: ["IIS 8.0", "GPO", "Permissions", "IIS 7.5", "Security", "IIS 7"]
aliases:
  - "/2013/07/sharepoint-iis-503-errors-and-gpos.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

One of the great things about my job is that I get to spend a bunch of time solving puzzles and fixing peoples' problems. I get to install SQL Server and SharePoint in existing, well established Federal environments. Incase you are unaware, in a segregated, secured environment, such as a Federal Agency, the left hand does not always talk to the right hand. The DNS team works apart from the Exchange Team, who works apart from the AD team, who is isolated from Network Ops team, who works independently from the firewall team, and so forth. Getting things accomplished in such an arena, can be challenging, and usually involves a vertical assent and decent approach, meaning I talk to my counterpart, they talk to their boss, who talks to, for example the AD Team's boss, and then my service accounts get created. 

**Scenario**

I was working with a group out of Alaska and Seattle, WA, to get a SharePoint 2010 Enterprise environment with SQL Server 2008R2 up and running after the group finished a domain migration. 

**SQL Issues**

As with any SharePoint Farm installation, I installed SQL first. Everything seemed to have installed correctly, however after reboot, the SQL Server instance and the SQL Agent instance would not fire up, as seen in Figure 1.

[![](/images/SQL Stopped.png)](/images/SQL Stopped.png)
**Figure 1**: Showing the stopped SQL Server and SQL Agent accounts.

In the process of troubleshooting, I opened up the service instance for the SQL Server Service and verified that the password was correct. As seen in Figure 2, after entering the account password, I was greeted with a message.

[![](/images/SQL Login Granted.png)](/images/SQL Login Granted.png)
**Figure 2**: Shows that after entering the account password, that the account has been granted the Log On As A Service right.

This pointed me in the right direction for the first problem in this environment. The service accounts that were created in Active Directory (AD) had a GPO rule that new accounts could not Log On As A Service. The new accounts were put into a new Security Group, ran a gpupdate /force ([http://technet.microsoft.com/en-us/library/bb490983.aspx](http://technet.microsoft.com/en-us/library/bb490983.aspx)), rebooted the server and SQL was now able to be rebooted successfully and have the service instances come up running!

**SharePoint Issues**

Unfortunately, placing all the service accounts into the new Security Group did not stop all the issues within the Farm. SharePoint installed correctly, and Central Administration(CA) provisioned and started correctly, however, after rebooting the server, I would receive a 503 error when trying to get to CA. Typically you would receive a 503 error when the Application Pool Account for your site has been stopped. After manually starting the account and clicking on the CA link, I would get a 503 error and my Application Pool Accounts would be stopped again, as seen in Figure 3.

[![](/images/IIS Stopped.png)](/images/IIS Stopped.png)
**Figure 3**: After browsing through the IIS sites, the Application Pools would stop.

Which brought up the following in the error logs:

Log Name:      System

Source:        Microsoft-Windows-WAS

Date:          6/28/2013 7:40:21 AM

Event ID:      5021

Task Category: None

Level:         Warning

Keywords:      Classic

User:          N/A

Computer:      

Description:

The identity of application pool SharePoint Central
Administration v4 is invalid. The user name or password that is specified for
the identity may be incorrect, or the user may not have batch logon rights. If
the identity is not corrected, the application pool will be disabled when the
application pool receives its first request.  If batch logon rights are
causing the problem, the identity in the IIS configuration store must be
changed after rights have been granted before Windows Process Activation
Service (WAS) can retry the logon. If the identity remains invalid after the
first request for the application pool is processed, the application pool will
be disabled. The data field contains the error number.

Event Xml: (removed)

Pretty nice to actually get a message that means something for once. But which account needs the Batch Login Rights? This is Federal Environment, I just cannot ask for all of my accounts to be put into a different GPO. After reading this article on IIS 6 [http://technet.microsoft.com/en-us/library/cc179801.aspx](http://technet.microsoft.com/en-us/library/cc179801.aspx) and this article that has the same issue, 

[http://talesfromitside.wordpress.com/2011/12/06/log-on-as-a-batch-job-permissions-removed/](http://talesfromitside.wordpress.com/2011/12/06/log-on-as-a-batch-job-permissions-removed/) 
, I was a little bit closer. Finally I read this KB article from Microsoft that really solved the problem: 

[http://support.microsoft.com/kb/981949](http://support.microsoft.com/kb/981949) (see Figure 4)

[![](/images/Log On As Batch Job.png)](/images/Log On As Batch Job.png)

**Figure 4:** Shows the default permissions and user rights for IIS 7.0, IIS 7.5, and IIS 8.0

So, after having the security policy modified to allow the IIS_IUSRS to Log On As A Batch Job, we ran a gpupdate /force, rebooted the server and the IIS Application Pool was able to stay up and running after trying to access Central Administration.

Update 12/29/2014

Recently, while working with a customer, I ran into interesting issue with my content service account (eg: spContent). I was able to provision my farm, create all of my service accounts, and create my Web Application and Site Collection. I was able to open Central Administration, however, when I tried to browse to the Site Collection URL, I would receive an error.

It was a bit odd that everything else worked, but the web page would not render. 

Now, I was in an environment known for having issues with GPO and had a bunch of devices in the background doing all kinds of packet inspection, so I was a bit hesitant as to what could be the error.

Luckily I did find a very helpful blog post: [SharePoint 2013: An exception occurred when trying to establish endpoint for context: Could not load file or assembly...](http://social.technet.microsoft.com/wiki/contents/articles/24494.sharepoint-2013-an-exception-occurred-when-trying-to-establish-endpoint-for-context-could-not-load-file-or-assembly.aspx)

This article basically asks to make sure that ***Impersonate a client at authentication*** has local rights.

**Summary**

Make sure your Service Accounts can Log On As A Service and that your IIS_IUSRS are allowed to Log On As A Batch Job. You might need to have the Impersonate a client at authentication has local rights as well.

Make sure that you do an iisreset after you push your GPO updates (or reboot the server(s)).
