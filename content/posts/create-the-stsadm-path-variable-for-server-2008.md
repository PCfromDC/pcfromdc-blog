---
title: "Create the STSADM Path Variable for Server 2008"
date: 2011-01-19T20:04:00+0000
tags: ["Server 2008", "STSADM", "Path", "Variable Path"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

In my last blog I talk about adding the STSADM command path as the first line of your scripts.  However, you can just create a Variable Path that points to STSADM, and never worry about typing out it's location ever again from a command prompt.  Here is how to add a path variable in Server 2008.

- Start menu --> right-click Computer --> Properties.
- From the Properties window, click Advanced system settings.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVrmcjAkTdmoMxDkWhEpRf_QJeCv_2oaVnR-xPbv5f0yWLAvpZApncDZPd81Z735EhdVj7TaPzQfvnszt4v7ZLB5Jht_cRjzuF8dXiFnG3INX152XSi8HEfWaRmybXk0GlKezyO4kTxOfy/s320/Computer+Properties.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjVrmcjAkTdmoMxDkWhEpRf_QJeCv_2oaVnR-xPbv5f0yWLAvpZApncDZPd81Z735EhdVj7TaPzQfvnszt4v7ZLB5Jht_cRjzuF8dXiFnG3INX152XSi8HEfWaRmybXk0GlKezyO4kTxOfy/s1600/Computer+Properties.jpg)
- Click Environment Variables.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRaXdRn7OV_8GXU4heZSnIfudU3JC5v7K3lAordng8ENJuaT895z-SmrxITEfOIAsBo-A9qizbvCDLlTOmrfgYUddXSRq-xYJX2-zGBIlAz6Ys3cUy3VCEcaRNqt2vFuu58pmMDG_yIgOX/s320/System+Properties.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRaXdRn7OV_8GXU4heZSnIfudU3JC5v7K3lAordng8ENJuaT895z-SmrxITEfOIAsBo-A9qizbvCDLlTOmrfgYUddXSRq-xYJX2-zGBIlAz6Ys3cUy3VCEcaRNqt2vFuu58pmMDG_yIgOX/s1600/System+Properties.jpg)
- Under System variables, scroll down and select the Path Variable --> Edit.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIP_0C80PkqOAXogNbs16W_nztIrwkuuCOw8PiegyeAq6iRP3gI_9GkFUqV99-DmGDOp5aCNymA2iDH-bJexiALdGT-dQ2g2kPT9rqeCLbII6Ixi5qaWwWHwjogcPX6b2LyVY4-TIsfUeM/s320/Environment+Variables.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgIP_0C80PkqOAXogNbs16W_nztIrwkuuCOw8PiegyeAq6iRP3gI_9GkFUqV99-DmGDOp5aCNymA2iDH-bJexiALdGT-dQ2g2kPT9rqeCLbII6Ixi5qaWwWHwjogcPX6b2LyVY4-TIsfUeM/s1600/Environment+Variables.jpg)
- Add a semi-colon (;) at the end of the last Variable value and paste in the path to STSADM:

C:\Program Files\Common Files\Microsoft Shared\web server extensions\12\BIN

       [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjcu3HKRH5j-M6Dyk76wTb8YNIhdtKCClyTuufdeHdzbwsDStov2Qn4u14_vRPx4iSFvXgyE23gBQ7uXvYKqfibHNd3enPmpnlpyYjsNliqThi6103_m2ddu1KhzsRIHUI1VHUJ6urk26w/s320/System+Variable.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjcu3HKRH5j-M6Dyk76wTb8YNIhdtKCClyTuufdeHdzbwsDStov2Qn4u14_vRPx4iSFvXgyE23gBQ7uXvYKqfibHNd3enPmpnlpyYjsNliqThi6103_m2ddu1KhzsRIHUI1VHUJ6urk26w/s1600/System+Variable.jpg)
