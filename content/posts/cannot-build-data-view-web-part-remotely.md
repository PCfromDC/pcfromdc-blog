---
title: "Cannot Build Data View Web Part Remotely"
date: 2011-02-05T04:02:00+0000
tags: ["SharePoint Designer", "Web Config"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I was trying to build a Data View Table using SharePoint Designer 2007 remotely.  I could create my Data Source and see all of my columns.  However, when I clicked to view the Data Source Details, I received an error: **
"The server returned a non-specific error when trying to get data from the data source. Check the format and content of your query and try again. If the problem persists, contact the server administrator."

After doing a little research, all I needed to do was modify the SqlDataSource SafeControlAssembly in the web.config file.SafeControl Assembly="System.Web, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" Namespace="System.Web.UI.WebControls" TypeName="SqlDataSource" Safe="****true****"** AllowRemoteDesigner=**"****true" ****

**I made the change, and now I can build my Data View Web Part from home, over the web, without a VPN!
