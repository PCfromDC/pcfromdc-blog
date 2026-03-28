---
title: "Determine Max Users From Requests Per Second (RPS)"
date: 2013-02-07T01:06:00+0000
categories: ["SharePoint"]
tags: ["SharePoint Farm", "Architecture", "RPS", "Templates", "Load Testing"]
aliases:
  - "/2013/02/determine-max-users-from-requests-per.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

While doing some work for a client the other day, I needed to reverse engineer how many users can their farm handle based on the results from a Visual Studio Load Test. Determining the requests per second that the users will generate is easy once you plug in the required information based off of the end users' usage profiles.

[![](/images/RPS- Clean.png)](/images/RPS- Clean.png)

Great!  According to the spreadsheet, their web server needs to be able to handle around 208 requests per second.  But if you run a load test on their farm, and you are given an Avg RPS, how many people can your farm handle based off the give utilization statistics?  This was a bit harder to figure out, but eventually I got there.  After plugging in the actual Avg RPS that the farm could handle while the hardware was still in it's "Green Zone" was 11.  When you put that into the spreadsheet, the number of Total Users that should access their environment should be less than 1,320.

[![](/images/Get User Count.png)](/images/Get User Count.png)

﻿

You can download the spreadsheet from here: [http://sdrv.ms/Xp7Sq0](http://sdrv.ms/Xp7Sq0)
