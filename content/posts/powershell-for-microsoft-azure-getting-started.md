---
title: "PowerShell for Microsoft Azure- Getting Started"
date: 2015-01-16T04:23:00+0000
categories: ["PowerShell", "Azure"]
tags: ["Getting Started", "PowerShell v5", "Introduction", "Infrastructure"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

This is part 2 of the blog series on getting Started with Microsoft Azure.

Part 1: [Microsoft Azure- Getting Started](http://pcfromdc.blogspot.com/2014/12/microsoft-azure-getting-started.html)

Part 2: PowerShell for Microsoft Azure- Getting Started  (This post)

Part 3: [Microsoft Azure- Create Geo Redundancy and Virtual Networks](http://pcfromdc.blogspot.com/2015/01/microsoft-azure-create-geo-redundancy.html)

Part 4: [PowerShell for Microsoft Azure- Creating Storage](http://pcfromdc.blogspot.com/2015/01/powershell-for-microsoft-azure-creating.html) 

Part 5: PowerShell for Microsoft Azure- Upload VHD Images (In Progress)

Part 6: PowerShell for Microsoft Azure- Create Machines (Coming soon)

Part 7: Active Directory and DNS in the Cloud and Azure AD (Coming Soon)

----------

Out of the box, you have the ability control Azure via a GUI (through your browser) or through PowerShell. The good part is that both options are free. Having the ability to control your Azure environment through PowerShell is a pretty amazing feature, and continues to prove that Microsoft is heavily vested in PowerShell. If you thing that PowerShell is just a fad and you are waiting for it to go away so that you don't have to learn it, good luck!

#### 
Let's Get Started

The first thing that is required is to download the Azure module for PowerShell. Personally, I prefer to go through the Web Platform Installer (WPI). The current version of WPI is 5, and can be downloaded from [http://www.microsoft.com/web/downloads/platform.aspx](http://www.microsoft.com/web/downloads/platform.aspx). Go ahead and run the installation and once WPI opens up, search for Microsoft Azure PowerShell. Depending on what you want to do, you may wish to install just PowerShell for Azure or you may choose to install the SDK as well. For this example, we are going to install Microsoft Azure PowerShell (Standalone) and Visual Studio Community 2013 with Microsoft Azure SDK- 2,5.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhK_h7YBTbAitSyJfLqY1RFeYtG0TmnvnlMwLwsnJYTTfXVCBD7LsH0UUhDJ_sVKYwjtoM0lX7UbupLXUI5F8Qle_29NOyyMFybyu4IJTjWVygiBltylHS63fVlqKQkeLviQTzNZ4i9PPgS/s1600/1-+install.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhK_h7YBTbAitSyJfLqY1RFeYtG0TmnvnlMwLwsnJYTTfXVCBD7LsH0UUhDJ_sVKYwjtoM0lX7UbupLXUI5F8Qle_29NOyyMFybyu4IJTjWVygiBltylHS63fVlqKQkeLviQTzNZ4i9PPgS/s1600/1-+install.png)

#### 
Open Up PowerShell ISE

For this demonstration, we will be using PowerShell ISE v5 on Windows 10 (beta). 

After opening ISE as Administrator, go to the commands add-on section and make sure that the Azure Module is installed.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZz6__O0ylrqkEJPFlUeLw4bhXeqN0d2wgJpLQzkp7voUB8Ne2FAcns07QQgyPBU3hZPjY26Sx59A1nI1OC2tcJZ31q16Z-e5aZs7WJFfB_nqdfRM7zNoLBM2KJoE3dgkhusZfONzIaHBi/s1600/2-+Validate.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZz6__O0ylrqkEJPFlUeLw4bhXeqN0d2wgJpLQzkp7voUB8Ne2FAcns07QQgyPBU3hZPjY26Sx59A1nI1OC2tcJZ31q16Z-e5aZs7WJFfB_nqdfRM7zNoLBM2KJoE3dgkhusZfONzIaHBi/s1600/2-+Validate.png)

Typically, when I work with PowerShell, the first thing that I will do is create a Scripts folder on a local drive to store my scripts, however we will be using the Scrips folder for storing passwords as well. 

#### 
Log In to Azure

There are several options for logging into Azure through PowerShell. However, once you have added your account into PowerShell, your credentials should stay. The most basic way to add your credentials to PowerShell is to use:

```
Add-AzureAccount
```

The problem is that this will open a windows login window will pop-up, and you enter your account name and password. This might actually be a good option for you if your Microsoft Account is the same as your O365 Account.

If you are not using a Microsoft Account to authenticate, another way to log-in, is to create a script that uses your stored username and password to authenticate. This will not work for @hotmail.com or @outlook.com accounts.

```
$userName = "pcBlogDemo@outlook.com"
$passwordLocation = "C:\Scripts\AzurePWString.txt"
function createPassword {
    Write-Host "Enter Password for $userName" -ForegroundColor Cyan
    Write-Host
    $PWInput = Read-Host -AsSecureString | ConvertFrom-SecureString
    $PWInput | Out-File $passwordLocation -Force
}
createPassword # Comment out this line after saving Password
$securePassword = Get-Content $passwordLocation | ConvertTo-SecureString
$cred = New-Object System.Management.Automation.PSCredential($userName, $securePassword)
Add-AzureAccount -Credential $cred

```

A final way is to download the Azure Publish Setting File, and log in with those credentials (certificate). There is a bit more work involved because you have to download and import the file first. Per Microsoft ([http://msdn.microsoft.com/en-us/library/dn495224.aspx](http://msdn.microsoft.com/en-us/library/dn495224.aspx)),"The publishsettings file contains the credentials (unencoded) that are used to administer your Windows Azure subscriptions and services. The security best practice for this file is to store it temporarily outside your source directories (for example in the Libraries\Documents folder), and then delete it after the settings have been imported. A malicious user gaining access to the publishsettings file can edit, create, and delete your Windows Azure services." To download the publishsettings file run:

```
Get-AzurePublishSettingsFile
```

This will open up a web page that will start a download and continue with instructions to finish importing the file. For this example, the document will be stored in the c:\Scripts folder.

After the file has been downloaded, run the Import-AzurePublishSettingsFile:

```
Import-AzurePublishSettingsFile "C:\scripts\Contoyso Azure-1-16-2015-credentials.publishsettings"
```

To validate the account running the PowerShell commands, run

```
Get-AzureAccount
```

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiMBpDLZaZD1uCURbGmfgmr2Ji0fZb9C9PT5EY28_PFKxSu6vjc-539DDyoUFCDvzM2RmItmb7rn7GLV38ZUMKnbI0mFqwvIUrjIppaV5jPnSqQVbYmUb_6-InNTo81n37PD9JkdB_KerwZ/s1600/3-+Get+1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiMBpDLZaZD1uCURbGmfgmr2Ji0fZb9C9PT5EY28_PFKxSu6vjc-539DDyoUFCDvzM2RmItmb7rn7GLV38ZUMKnbI0mFqwvIUrjIppaV5jPnSqQVbYmUb_6-InNTo81n37PD9JkdB_KerwZ/s1600/3-+Get+1.png)

If you have multiple Azure subscriptions, you will have to set the Azure subscription that you are working on, so that you do not accidentally do work in the wrong place.

```
Select-AzureSubscription -SubscriptionName "Contoyso Azure"
```

#### 
Conclusion

You have learned several ways (good and bad) to log into Azure. At this point you should now be able to log in and run PowerShell against your tenant. 

#### 
What's Next

Your options on what to do within Azure are truly limitless as long as you stay within your $200 free trail or within the budget of your organization. Most organizations start simple using Azure for High Availability and Disaster Recovery of AD and DNS services. So, the next steps would be to map out what data centers you plan on utilizing to host your servers and document a network class and IP range for each region. You can find information about data center locations at [http://azure.microsoft.com/en-us/regions/](http://azure.microsoft.com/en-us/regions/)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_hfE0KSCoMQeO0BUgf2PgPLDSIkx0IYeFz8mBmp1Z4ZrM-cA2WrHZm2JmLOjsaj1oSutgWwzfmq0YLuzy38WVXHuoAGKm88icIO_1B1ukgqFebWEeQYCYnefPUG22j76ZFDm4yJ8uq9dl/s1600/3-+data+center+locations.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_hfE0KSCoMQeO0BUgf2PgPLDSIkx0IYeFz8mBmp1Z4ZrM-cA2WrHZm2JmLOjsaj1oSutgWwzfmq0YLuzy38WVXHuoAGKm88icIO_1B1ukgqFebWEeQYCYnefPUG22j76ZFDm4yJ8uq9dl/s1600/3-+data+center+locations.png)

#### 
Geo Redundant Strategy and Subnets

Since Contoyso has determined that it will be using Azure for high availability and disaster recover for Active Directory and DNS, here is a map of the data centers chosen and IP ranges selected.. 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjAQ93Kl_2yWZpho9EA0V7c6-6lWcvRImCembCRBqicSJ_QV1SaMLOKEUfv16UhvZ6Ragsfgio1hpfTL1xcA128oGKZYLTtlKkEYqMTeIV_aqgLN9CaTbQPCg6d2IAzaESZ-etIWahipIvB/s1600/4-+Azure+Infrastructure.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjAQ93Kl_2yWZpho9EA0V7c6-6lWcvRImCembCRBqicSJ_QV1SaMLOKEUfv16UhvZ6Ragsfgio1hpfTL1xcA128oGKZYLTtlKkEYqMTeIV_aqgLN9CaTbQPCg6d2IAzaESZ-etIWahipIvB/s1600/4-+Azure+Infrastructure.png)

Having your mappings done ahead of time will make life easier when it comes to configuring your networks.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwXXkNlr8GNv3rzduroTcerSqJ7ijyuF10PNDMNAfoFhiy1LiHvm8XgN1G05dSsWH5Re4UKaDajhfH6XjsVpB5iEB7W4X1eWuIt7PMBvqMMfScr69eF-cKOmDjha7ve_7AoMoMOXlon_82/s1600/8-+Network+Info.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgwXXkNlr8GNv3rzduroTcerSqJ7ijyuF10PNDMNAfoFhiy1LiHvm8XgN1G05dSsWH5Re4UKaDajhfH6XjsVpB5iEB7W4X1eWuIt7PMBvqMMfScr69eF-cKOmDjha7ve_7AoMoMOXlon_82/s1600/8-+Network+Info.png)
You can download the Excel file from [http://1drv.ms/1xvEfcW](http://1drv.ms/1xvEfcW)

Stay tuned for the next blog post on creating geo-redundancy and virtual network in Azure (the picture above)...
