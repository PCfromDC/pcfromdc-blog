---
title: "Centering Your SharePoint 2010 Master Page"
date: 2010-07-17T13:16:00+0000
aliases:
  - "/2010/07/centering-your-sharepoint-2010-master.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

The other day, I was working on creating a new master page, starting with the v4.master. The team that I was working with asked to have the page centered in the browser window, and they sent me over a link to SharePoint Blues to use as reference.

[http://www.sharepointblues.com/2010/05/06/centered-layout-with-v4-master/](http://www.sharepointblues.com/2010/05/06/centered-layout-with-v4-master/)

Unfortunately, it did not work in my masterpage, so we kept looking and my coworker Ben found a simple solution:

- Open up your masterpager
- Look for the start of the body 
- Insert the following within the body:

-  {margin: 0 auto;}

And that was all that it took to fix the issue...  gotta love simple...

[![](/images/Marging 0.png)](/images/Marging 0.png)
