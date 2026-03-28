---
title: "Windows 8 and The Unsupported Cisco VPN Client"
date: 2012-08-11T21:24:00+0000
tags: ["Windows 8", "Cisco", "Registry Hacks", "VPN", "VPN Client"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

When you spend most of your time doing remote sessions into client environments using the Cisco VPN Client, I found it a bit troubling to find out that [Cisco announced their VPN Client End of Life](http://www.cisco.com/en/US/prod/collateral/vpndevc/ps5743/ps5699/ps2308/end_of_life_c51-680819.html).  However, since I had the installation on a supported Windows 7 box, I was not too worried that Windows 8 would not be supported.  Fast forward to August 8, 2012: my laptop died, all I had was a Windows 8 tablet to use as my defunct work computer, and I was no longer able to get to my clients...

To resolve this issue:

1) Open up the Registry Editor:

\HKLM\SYSTEM\CurrentControlSet\Services\vpnva\DisplayName

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEilqQcweYSqnmGhyphenhyphenyHmWEe6kmNy2iQAyvcTLozMqlxcmobGrjssefBDYX2NfHObpxiMsoFwBsoB0bJoPufvIfcOgAdCjDnx4b9-XK_FjtOQtZUiwfGjwS5voFUKV669W10gk3LyfB7xhgjF/s400/Regedit.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEilqQcweYSqnmGhyphenhyphenyHmWEe6kmNy2iQAyvcTLozMqlxcmobGrjssefBDYX2NfHObpxiMsoFwBsoB0bJoPufvIfcOgAdCjDnx4b9-XK_FjtOQtZUiwfGjwS5voFUKV669W10gk3LyfB7xhgjF/s1600/Regedit.PNG)

2) Edit the DisplayName, and clean out all of the junk (including the semi-colon) before the adapter name, leaving just the display name.  For example:

"@oem10.inf,%vpnva_Desc%;Cisco AnyConnect VPN Virtual Miniport Adapter for Windows x64"

becomes:

"Cisco AnyConnect VPN Virtual Miniport Adapter for Windows x64"

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgTQeP2QTrmeqlO2y5U9d9VXlxd1EOCFEGnR7yX-yXTCbCfHKhULbV2GEvD-qpT5kIQo3by-L1flWcFWZZ_OWuwLGXA2r6Vvs-td3F-DsXbuJ5PTV7BHuJjFo9hpSw-BG4ERMOLjjVU1cDw/s400/DisplayName.PNG)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgTQeP2QTrmeqlO2y5U9d9VXlxd1EOCFEGnR7yX-yXTCbCfHKhULbV2GEvD-qpT5kIQo3by-L1flWcFWZZ_OWuwLGXA2r6Vvs-td3F-DsXbuJ5PTV7BHuJjFo9hpSw-BG4ERMOLjjVU1cDw/s1600/DisplayName.PNG)
