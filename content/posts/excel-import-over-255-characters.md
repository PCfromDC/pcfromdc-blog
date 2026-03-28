---
title: "Excel Import Over 255 Characters"
date: 2011-02-09T01:27:00+0000
tags: ["Troubleshooting", "Excel"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Another Excel gotcha!!!  I have a SQL column called myText and it is a varchar(2500), but when I tried to import the appropriate cell, I kept getting null values returned.  If my cell was 255 characters or less, I would be fine.  The first hint was in the sample preview for cell formating.  Anytime I saw hash marks in the sample box, my text would not import.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgeTCMHO02iDHmiZOhkWIIBIxRr4FVn_-N19gN8vCGFmzxG3-VbV5Dx4tuYdpF4frWCyHjEe11EjtP2AKHIcM9fK4I6U6l3dipgOh0-06upEQM_psiMHEEa2YAtQK5gAbY-eBSWoi9MzMoS/s640/Hint+of+Problem.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgeTCMHO02iDHmiZOhkWIIBIxRr4FVn_-N19gN8vCGFmzxG3-VbV5Dx4tuYdpF4frWCyHjEe11EjtP2AKHIcM9fK4I6U6l3dipgOh0-06upEQM_psiMHEEa2YAtQK5gAbY-eBSWoi9MzMoS/s1600/Hint+of+Problem.jpg)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEicuRLpwxnXB9DAR499Oa0XFYD4RhiPAaHKdbhM9dyd3SXyWbbzW19pzfngAKPZAdgGwSeP8fh5p_kJD2za89-a5N3oxccQK9ZxVIdjXEEpyasWOpEkG60Nz8PkrmiIcf3k_obMYqW_guw0/s640/Another+Hint+of+Problem.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEicuRLpwxnXB9DAR499Oa0XFYD4RhiPAaHKdbhM9dyd3SXyWbbzW19pzfngAKPZAdgGwSeP8fh5p_kJD2za89-a5N3oxccQK9ZxVIdjXEEpyasWOpEkG60Nz8PkrmiIcf3k_obMYqW_guw0/s1600/Another+Hint+of+Problem.jpg)

Here is how I fixed it!  Added a Custom "text" Type, and all the hash marks disappeared!

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRtXufx9oDEOLgr90c5vSk42kBkHH9ffYZj1uuI6liZZ4w5FfWUqizHjeMLyk-TGNiYazMvCvc9RwEQjmoo7yN92h_-wGJjscVLAiEPCbq9G6_gRBbxpYAhI2xXM4BL6nSE-vity_AnAMf/s640/Set+text+Format.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgRtXufx9oDEOLgr90c5vSk42kBkHH9ffYZj1uuI6liZZ4w5FfWUqizHjeMLyk-TGNiYazMvCvc9RwEQjmoo7yN92h_-wGJjscVLAiEPCbq9G6_gRBbxpYAhI2xXM4BL6nSE-vity_AnAMf/s1600/Set+text+Format.jpg)
