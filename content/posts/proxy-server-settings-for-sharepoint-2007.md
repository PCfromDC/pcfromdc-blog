---
title: "Proxy Server Settings for SharePoint 2007"
date: 2010-04-21T21:12:00+0000
categories: ["SharePoint"]
tags: ["Web Config", "Proxy Settings"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

We do not have any "Content" deployment people at my office so I depend mostly on RSS feeds to keep the sites updated and changing on a constant basis.

One day, our IT department put in a Proxy Server, which killed all of the RSS feeds to SharePoint.  I had to update the config file so that the RSS feeds were available once again.

I used the following sites as reference...

[http://geekswithblogs.net/hinshelm/archive/2007/10/24/Proxy-server-settings-for-SharePoint-2007.aspx](http://geekswithblogs.net/hinshelm/archive/2007/10/24/Proxy-server-settings-for-SharePoint-2007.aspx)

[http://microsoft-sharepoint-services.blogspot.com/2008/12/configuring-proxy-setting-on-sharepoint.html](http://microsoft-sharepoint-services.blogspot.com/2008/12/configuring-proxy-setting-on-sharepoint.html)

[http://support.microsoft.com/kb/912060](http://support.microsoft.com/kb/912060)

You will need to put the Proxy Setting into each Virtual Directory for every site that you want to be able to access the Internet.

The main reason that I am writing about these settings is that it took me too long to figure out that the last slash in the "proxyaddress" is REQUIRED...

In Server 2008, you will find the required files here... 

C:\inetpub\wwwroot\wss\VirtualDirectories\YOUR SITE NAME\web.config

Add the following to the end of the .config file:

<system.net>

  <defaultproxy>

     <proxy usesystemdefault="false" proxyaddress="http://x.x.24.45:3128/" bypassonlocal="true" />

  </defaultproxy>

</system.net>
