---
title: "Excel Import Over 255 Characters"
date: 2011-02-09T01:27:00+0000
tags: ["Troubleshooting", "Excel"]
aliases:
  - "/2011/02/excel-import-over-255-characters.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Another Excel gotcha!!!  I have a SQL column called myText and it is a varchar(2500), but when I tried to import the appropriate cell, I kept getting null values returned.  If my cell was 255 characters or less, I would be fine.  The first hint was in the sample preview for cell formating.  Anytime I saw hash marks in the sample box, my text would not import.

[![](/images/Hint of Problem.jpg)](/images/Hint of Problem.jpg)

[![](/images/Another Hint of Problem.jpg)](/images/Another Hint of Problem.jpg)

Here is how I fixed it!  Added a Custom "text" Type, and all the hash marks disappeared!

[![](/images/Set text Format.jpg)](/images/Set text Format.jpg)
