---
title: "Force .eml Files to Open in Outlook 2007"
date: 2011-08-10T18:49:00+0000
categories: ["SharePoint"]
tags: ["Email", "Registry Hacks", "Outlook"]
aliases:
  - "/2011/08/force-eml-files-to-open-in-outlook-2007.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

# Background
Files being collected in SharePoint email enabled lists are being received as .eml files by default since SharePoint uses SMTP services for receiving email.  The problem is that people want to use Outlook, not Outlook Exprerss to view their emails, and .eml files are not native to Outlook 2007 or earlier.
# Workaround
1)      Modify the client registry:a.     Make a backup of the following eml-file registration:                                                               i.       HKEY_CLASSES_ROOT\.emlb.      Install appropriate eml-Outlook2007-xxx.reg file by double clicking the file.c.       Information taken from [http://www.msoutlook.info/question/354](http://www.msoutlook.info/question/354)2)      Set the .eml file default to open in Outlook 2007a.       Right click a .eml fileb.      Open With à Choose default program…c.       Choose Outlook.exe                                                               i.      C:\\Program Files\Microsoft Office\Office12\Outlook.exe3)      Modify the client registry again:a.     Make a backup of the following registration:                                                               i.       HKEY_CLASSES_ROOT\MIMEb.      Modify “HKEY_CLASSES_ROOT\MIME\Database\Content Type\message/rfc822”

extension=".eml"

CLSID=""c.       Information take from: 

[http://social.msdn.microsoft.com/Forums/en-US/vbgeneral/thread/d94c0d4e-0d32-4648-bdd6-dc3f28bb4797/](http://social.msdn.microsoft.com/Forums/en-US/vbgeneral/thread/d94c0d4e-0d32-4648-bdd6-dc3f28bb4797/)
