---
title: "PowerShell for SharePoint in O365- Getting Started"
date: 2013-08-08T03:16:00+0000
categories: ["PowerShell", "SharePoint"]
tags: ["ISE", "SharePoint Online", "O365"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Getting PowerShell for SharePoint Online up and running is relatively easy, however, you might have to download a couple of things. And if you are new to PowerShell, you could be wondering what can you actually do once you have the SharePoint Online Management Shell installed? So here is a beginners guide on how to get things started on your local machine.**

1)** Make sure that you have installed Windows PowerShell 3.0. If you do not have PowerShell 3.0, you will need to download the [Windows Management Framework 3.0](http://www.microsoft.com/en-us/download/details.aspx?id=34595)**
2)** You will need to install the SharePoint Online Management Shell, which can be downloaded from the [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=35588)**
3)** Run PowerShell, Windows PowerShell, the new SharePoint Online Management Shell or the Windows Integrated Scripting Environment (ISE).**
As seen in Figure 1**, you can find your new Shell by searching for it. However which ever tool you decide to run, PowerShell, ISE or the Online Shell, you will need to run it as an Administrator. If you do not, you will receive an error, as seen in **Figure 2**.**
[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiESD88Rp2d-Yj-boctvFrG7czkN5PWLnkN-VO85wvpeV6CgQy62obKF5dvdOMMCKDrG_zLuYwkUUgC-U6_cRzxK0RBcxhLAWHWOwUIkxScWRn5rRa017BQi46d6zrkvSf9HYBKRL7kujCz/s1600/SharePoint+Online.png) 

Figure 1:** Searching for the new Online Management Shell**

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuTmzgxLK7oz1gWuKxWd8D_y9AUBYutsM-UAihRJyzTQ5lTb57JIGEQimCXC26Tr942Hznb__L53KkZm1yYVR-SMEWL2Hm2p_N5uKsnjX0nvjrsOfWLjq8VlTC7dEeuxtezVDG7shOcUHS/s640/Open+Error.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhuTmzgxLK7oz1gWuKxWd8D_y9AUBYutsM-UAihRJyzTQ5lTb57JIGEQimCXC26Tr942Hznb__L53KkZm1yYVR-SMEWL2Hm2p_N5uKsnjX0nvjrsOfWLjq8VlTC7dEeuxtezVDG7shOcUHS/s1600/Open+Error.png)

Figure 2:** You will need to Run As Administrator to avoid this error**

If you take a close look at the error in Figure 2**, notice the Import-Module cmdlet that is used to import the Microsoft.Online.SharePoint.PowerShell module. The cmdlet is using the "DisableNameChecking" parameter; this is because of the use of non-standard verbs. In **Figure 3**, you can see that the "Upgrade-SPOSite" would be the non-standard verb. You can view the imported cmdlets (verb-noun) by running:**

```
Import-Module Microsoft.Online.SharePoint.Powershell -Verbose
```

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjeYzbhocAF5UZXE_08MP1nwy9hhZddDm-66xVZ4o3HB67l1__LnXnAMRRr9FH10uVXhPUm-C6enXBPA33uQP4CT0qPgnJziL9Nn2lbqZfIMY45ckbBEyBcftkjjrS2M066nLaouvucGnel/s640/Non+Standard+Verb.png)

Figure 3:** Shows the non-standard verb

**

In my Windows 8 deployment, the module was automatically added, as seen in Figure 4**.**
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEigvQvK23TgZ0rt2fV2R0-VXiCPN6iyisiXS4XxugxI1dZOGnFADS3_eufk1NbELp5BJj1po8Lm8nI-O9cKcEFZHoQ40vF9qPBkzhwY26mrKULx6EILdwHvddQGDQQaOqn00W4odoxUqfob/s400/PowerShell+Module+Added.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEigvQvK23TgZ0rt2fV2R0-VXiCPN6iyisiXS4XxugxI1dZOGnFADS3_eufk1NbELp5BJj1po8Lm8nI-O9cKcEFZHoQ40vF9qPBkzhwY26mrKULx6EILdwHvddQGDQQaOqn00W4odoxUqfob/s1600/PowerShell+Module+Added.png)

Figure 4:** Using ISE, you can verify installation by looking in the Modules drop-down.**

One of the great features of running ISE is that users are able to see all the commands available to them. Figure 5 **shows all of the commands (verb-noun) available for the Microsoft.Online.SharePoint.PowerShell module.**
[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYlBOryVcHW-FU_Xhh2NPW3BPcYwd9JysOxHqCLWc2BKS89nlgr9-S1ZXnSLW8BZKTYdIBZ4VKc0e57lpLXq1sQXI1lcEj9kbEykBJLcIW3I_6D8Y_qzMMV_-2MwB-wEOswBB8gCd1nLoW/s640/Available+Commands.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYlBOryVcHW-FU_Xhh2NPW3BPcYwd9JysOxHqCLWc2BKS89nlgr9-S1ZXnSLW8BZKTYdIBZ4VKc0e57lpLXq1sQXI1lcEj9kbEykBJLcIW3I_6D8Y_qzMMV_-2MwB-wEOswBB8gCd1nLoW/s1600/Available+Commands.png) 

Figure 5:** A list of all available cmdlets for the Online module**

4)** To be able to start using PowerShell online, you will need to connect to the admin site of your tenant.**

```
Connect-SPOService -Url https://yourTenant-admin.sharepoint.com -Credential username@yourTenant.com
```

5)** Once you are connected, take it for a test drive!**

```
Get-SPOUser -Site https://yourTenant.sharepoint.com
```

In Figure 6**, you can see the results and some interesting accounts used by O365 to help manage your site.**

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhpXAHfPkiZPHYDFoJf_2UkaOdIWLl7D6eASOuCUGkwHDP9LJxQuDOS1yyrR-Us1w7fPAoNM0lZ-5Edo9aPPhl6AW3EUqVF_j3Llju5A9e2EtPaFjCOV3BkWNUnzX-hlkhzabgQC2FU07PS/s640/SPO+Users.png)
**
**Figure 6:** The returned results from the Get-SPOUser cmdlet.**

Another benefit of using ISE over just the Management Shell,  is that the Commands tab within ISE will actually help me create my script by showing me the required and available parameters to fill out, as seen in Figure 7**.**
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj4hkcbJuJV9Pu__zqb05O4hcXRqqj86tI0beQzHgGpc7qmZB9OfiXUCZB51Jh0zAgpaJYodwWq_uFxFeyKZRaIRXnFCkZgP1wYjl8DZaMEE7GkWR5E18TjSFz5szIXIAzlkWPhoh-IqlTn/s400/Insert+command+and+parameters.png)

Figure 7**: ISE will show you the parameters available and will insert the command into the Script Pane.**

ISE also uses [Intellisense](http://blogs.msdn.com/b/powershell/archive/2012/06/13/intellisense-in-windows-powershell-ise-3-0.aspx), as seen in Figure 8**.**
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgarCrYpatC3TDdP5jCeOj_DsN0XzTLu94b76AviQuMBeigwMHonng1a3jqbpOqChNIOe0-LeUpMkdGeFCEzKQ5NLkYuv8HdLFsUEcQApYJCFJo6YJYPynm0VHg4iCGWsjiM3oCbb5az1Dx/s400/Auto+Complete.png)

Figure 8**: Shows the Intellisense parameters available for the Connect-SPOService cmdlet.

If you are still a bit timid to start writing your own commands, an excellent reference for helping you to create PowerShell commands, is Bill Baer's online Windows PowerShell for SharePoint Command Builder*. You can download the [instruction guide](http://www.microsoft.com/en-us/download/details.aspx?id=27588) or you can go straight to the [command builder](http://www.microsoft.com/resources/TechNet/en-us/Office/media/WindowsPowerShell/WindowsPowerShellCommandBuilder.html) website and start creating.

Update: 08/09/2013: Cleaned up verbiage due to late night blog posting...
