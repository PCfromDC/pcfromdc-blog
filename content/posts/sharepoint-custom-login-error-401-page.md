---
title: "SharePoint Custom Login Error (401) Page"
date: 2011-02-10T14:27:00+0000
categories: ["SharePoint"]
tags: ["Custom Page", "Login Error", "Web Config", "IIS 7"]
aliases:
  - "/2011/02/sharepoint-custom-login-error-401-page.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

When dealing with lots of people logging into your SharePoint site, you will spend a lot of time answering phone calls from people with login errors. A nice and easy way to preemptively help deal with login failures is to use a custom error page.

[![](/images/Message.jpg)](/images/Message.jpg)

1) Go to your IIS and look up where the custom errors are located for the site.

[![](/images/IIS Site.jpg)](/images/IIS Site.jpg)

2) Look at the path location for the location of the error files.[![](/images/error page locations.jpg)](/images/error page locations.jpg)

3) Go to the file location,, you will want to edit the 401.htm file.

[![](/images/errror pages.jpg)](/images/errror pages.jpg)

4) The last step is to modify the web.config for your SharePoint site.  You will need to add (modify) the system.webserver.[![](/images/Web Config.jpg)](/images/Web Config.jpg)

[Download code here](http://cid-5fd92a4c199af58e.office.live.com/view.aspx/401%20Error%20Code/401%20Error%20Code.docx)

To add custom error pages in different languages, just add the pages to the appropriate local language folder.  You can find the local language folder information here...  [http://msdn.microsoft.com/en-us/library/bb266177.aspx](http://msdn.microsoft.com/en-us/library/bb266177.aspx)
