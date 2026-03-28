---
title: "Increase Time Before Connection Timeout Between SQL and SharePoint"
date: 2011-03-18T03:05:00+0000
categories: ["SharePoint", "SQL"]
tags: ["Connections", "SharePoint 2010", "SharePoint 2007", "Web Config"]
aliases:
  - "/2011/03/increase-time-before-connection.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I have run into an issue with a couple of clients where a Connection Timeout Error occurs on either very large SSRS reports or in some custom web parts pulling data from SQL.   By default, the connection between SharePoint and your SQL servers will timeout after 120 seconds.  My current client just had me create a report that takes about 8.5 minutes to complete rendering...  oops, error!

This is how you fix the timeout issue:

1)  Go to the virtual directory for the site that is timing out.

         C:\inetpub\wwwroot\wss\VirtualDirectories\*yourSite*

2)  Make a backup of the web.config file.

3)  Edit the web.config file and add an httpRuntime property called "executionTimeout"

In the example below, my connection will now timeout after 5 minutes (300 seconds).

[![](/images/Increase Timeout.jpg)](/images/Increase Timeout.jpg)
