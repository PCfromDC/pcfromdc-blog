---
title: "Running STSADM Commands"
date: 2011-01-19T18:31:00+0000
categories: ["SharePoint"]
tags: ["STSADM", "Path"]
aliases:
  - "/2011/01/running-stsadm-commands.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I know that everyone and their mother has an opinion about how to deal with STSADM commands, so one more opinion won't hurt.

I suggest creating a folder on the C: drive called "Scripts", to store all of the batch (.bat) and command (.cmd) files that you create/run.  It is a lot easier to edit the files in Notepad than it is to retype the whole STSADM command when you make a mistake.  Also, if you blow up your farm, you have a record of all scripts run in case you have to rebuild.

The first thing to do when creating a script for SharePoint 2007 is to add the STSADM path as the first line:

cd C:\Program Files\Common Files\Microsoft Shared\web server extensions\12\BIN

Then write your STSADM command(s)

The last line of your script should be:

Pause

This will allow you to see the errors or successes without the cmd window closing.

Please remember to right-click the script and Run as Administrator.
