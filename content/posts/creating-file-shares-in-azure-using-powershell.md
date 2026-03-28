---
title: "Creating File Shares in Azure using PowerShell"
date: 2017-04-04T01:54:00+0000
categories: ["PowerShell", "Azure"]
tags: ["File Share", "Azure Storage"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

The other day I was helping migrate a client from one cloud provider into Azure, when they ran into a problem with their file share server. It made me think about the [Azure File Service](https://blogs.msdn.microsoft.com/windowsazurestorage/2014/05/12/introducing-microsoft-azure-file-service/) that is available, and how to implement this from a corporate perspective, and then my mind wandered and I wanted to see how to create a file share just for my Surface Pro. There is an excellent post called [Get started with Azure File storage on Windows](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-files) that go me started, but I was not too happy with the implementation, as I would like my file share available to me after I reboot my Surface Pro, plus I do not want anyone logged into my Surface to have access to MY file share.

The beginning of the script is basic creating the Resource Group and Storage Account, as it is a file share, I am using global redundant storage, on HDDs not SSDs.

Storage within Azure is Context based, so to create the Azure File Share, we will need to create a context. Once we create the context, we can create the File Share.

Now that we have created the Azure File Share, we want to store the login credentials locally to make mounting the drive easier.

The next step is to mount the file share as a local drive (X:\ drive in this example). I tried using the New-PSDrive cmdlet, but could not get it to work consistently, so ended with the script below. One thing to notice is that in line 9 I am creating a Script Block from a string variable, instead of creating a script block with parameters and passing in values. I found this to be a very nice and easy way to deal with passing variables into a script block. Plus I need the script block in a later part of the script, so it was win-win.

Now at this point, I have a mounted X Drive to my Azure File Share, but it is not persisted. To allow my drive to return after reboot, I have to create a Scheduled Job that will run on logon for the person running this script. 

Notice in line 15, I grab the Scheduled Job's Path so that I can grab and update the Scheduled Task to update the Task's principal user.

All in all a very cool, and quick way to add 5TB of storage to my Surface Pro.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEii5OBHuWSBe_SrjW8OlsFSad6hGavLCR1gMiI5HdiTeqMTgXNWFmJXb5Vv7gTaDwZUCFhZ1MCaEjsrYx_Z5kdWuGiPOZGSUBtd8m6Jgh-aXQstwu8wsOrQU1vRPxS7A_D79mD8kp9AQW7J/s400/X-Drive.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEii5OBHuWSBe_SrjW8OlsFSad6hGavLCR1gMiI5HdiTeqMTgXNWFmJXb5Vv7gTaDwZUCFhZ1MCaEjsrYx_Z5kdWuGiPOZGSUBtd8m6Jgh-aXQstwu8wsOrQU1vRPxS7A_D79mD8kp9AQW7J/s1600/X-Drive.png)
I could not think of any other reasons why an Enterprise would want a file share in Azure (from a corporate perspective) with the availability of SharePoint Online or OneDrive for Business, but then a client asked me how to send me their bloated SQL Database... You would send your client something like this:

Once they upload the file, you would then create a new key for the storage account.

How would/are you using Azure File Service?

Here is the code in its entirety:
