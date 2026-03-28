---
title: "IIS7 and Multiple IP Addresses That Won't Bind"
date: 2010-10-07T14:10:00+0000
tags: ["IP Binding", "Tomcat", "IIS 7"]
aliases:
  - "/2010/10/iis7-and-multiple-ip-addresses-that.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Recently one of my clients asked to put a Tomcat Server on the same box as their SharePoint 2007 WFE. 

1) Add the IP address (Local Area Connections --> Properties --> Advanced --> IP Address Add...)

[![](/images/LAC.jpg)](/images/LAC.jpg)[![](/images/LAC Properties.jpg)](/images/LAC Properties.jpg)[![](/images/LAC Properties Advanced.jpg)](/images/LAC Properties Advanced.jpg)[](/images/LAC Properties.jpg)

2) Stop IIS  (cmd --> iisreset -stop)3) Set binding information in Tomcat

4) Set binding information in IIS          (IIS Manager --> Right Click Site --> Edit Bindings --> Edit Port Binding --> Select IP from drop down) 

[![](/images/IIS.jpg)](/images/IIS.jpg)[![](/images/IIS Binding Edit.jpg)](/images/IIS Binding Edit.jpg)

[![](/images/IIS Binding Edit Site.jpg)](/images/IIS Binding Edit Site.jpg)

5) By default, IIS7 binds to ALL port 80 IPs, so we have to disable this behavior for the IP in IIS       (cmd --> netsh http add iplisten ipaddress=xxx.xxx.xxx.xxx)

[![](/images/netsh.jpg)](/images/netsh.jpg)[](/images/netsh.jpg)

6) Restart IIS (cmd -->  iisreset)

Warning!!!

If you have created Custom Web Services, running the Net Shell command will bind your Custom Web Services to the IP address and cause them to stop working.  This will give you an error of:  Not Able To Connect To Remote Server.
