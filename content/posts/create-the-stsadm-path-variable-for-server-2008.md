---
title: "Create the STSADM Path Variable for Server 2008"
date: 2011-01-19T20:04:00+0000
tags: ["Server 2008", "STSADM", "Path", "Variable Path"]
aliases:
  - "/2011/01/create-the-stsadm-path-variable-for.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

In my last blog I talk about adding the STSADM command path as the first line of your scripts.  However, you can just create a Variable Path that points to STSADM, and never worry about typing out it's location ever again from a command prompt.  Here is how to add a path variable in Server 2008.

- Start menu --> right-click Computer --> Properties.
- From the Properties window, click Advanced system settings.

[![](/images/Computer Properties.jpg)](/images/Computer Properties.jpg)
- Click Environment Variables.

[![](/images/System Properties.jpg)](/images/System Properties.jpg)
- Under System variables, scroll down and select the Path Variable --> Edit.

[![](/images/Environment Variables.jpg)](/images/Environment Variables.jpg)
- Add a semi-colon (;) at the end of the last Variable value and paste in the path to STSADM:

C:\Program Files\Common Files\Microsoft Shared\web server extensions\12\BIN

       [![](/images/System Variable.jpg)](/images/System Variable.jpg)
