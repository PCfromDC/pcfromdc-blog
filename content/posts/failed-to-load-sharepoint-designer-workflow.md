---
title: "Failed To Load SharePoint Designer Workflow"
date: 2010-10-28T20:49:00+0000
tags: ["SharePoint 2007", "Workflows", "SharePoint Designer"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

A client ask me to help troubleshoot one of their workflows.  The problem was an inconsistent issue; sometimes it worked, and sometimes it did not.  So I jumped on to my SPD 2007 to take a look at their workflow.  I could not see anything obvious in the workflow, logs and error reports.  It was getting late, so I called it a night.  The next morning I logged back into my SPD 2007 to see if I missed something on the workflow when I received the following error. 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqBVgHn7Tr1VVLaXaENwFIGllpU_yMF1Vcw336_6Z4YCB4qymxaVfJf3ZbOrjAlCznbGHvJkkxBuh8zdKsC9ahh9j2rYuixLPf69xS1OQuimVHsILlXZo-_F7gcHOb0ThZfrNfYGELX9HX/s320/Failed+Workflow.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqBVgHn7Tr1VVLaXaENwFIGllpU_yMF1Vcw336_6Z4YCB4qymxaVfJf3ZbOrjAlCznbGHvJkkxBuh8zdKsC9ahh9j2rYuixLPf69xS1OQuimVHsILlXZo-_F7gcHOb0ThZfrNfYGELX9HX/s1600/Failed+Workflow.png)

It worked last night, what has changed?  Well the client had recently set up a second web front end (WFE), and a network load balance (NLB) piece of hardware.  I had read through a couple of posts...
- [http://social.msdn.microsoft.com/Forums/en-US/sharepointworkflow/thread/9874845b-9bf0-4723-9142-7384bbbcb1a6](http://social.msdn.microsoft.com/Forums/en-US/sharepointworkflow/thread/9874845b-9bf0-4723-9142-7384bbbcb1a6)
- [http://blog.qumsieh.ca/2010/01/16/failed-to-load-workflow-error-in-sharepoint-designer/](http://blog.qumsieh.ca/2010/01/16/failed-to-load-workflow-error-in-sharepoint-designer/)

And while reading the second post, it hit me to check the web.config on the WFEs to make sure that they are identical. It turned out that I was missing an authorizedType Assembly in one of the web.config files.  After adding the line back in, all was happy again.
