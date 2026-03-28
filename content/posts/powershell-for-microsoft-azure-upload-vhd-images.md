---
title: "PowerShell for Microsoft Azure- Upload VHD Images"
date: 2015-04-04T19:01:00+0000
draft: true
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

#### 
Create Storage and Upload Your VHD

Why would you want to upload a VHD into Azure? The first reason that comes to mind is the "Golden Disk". A lot of the organizations that I deal with have a vetted master OS image that has passed a Security Team audit and has certain features removed, certain features enabled, tweaks to the Registry, along with specific security software already installed. If Azure is to be used as an Enterprise solution and add IaaS functionality to their network, having the ability to upload a "Golden Disk" would be very important. Another reason would be cost. Currently, if you deploy a Windows Server into Azure, you will be deploying a Data Center version of the Server OS. Who is paying for the licensing for Data Center? You are... The license to run Windows Server in Azure is included in the per-minute cost of the virtual machine ([http://azure.microsoft.com/en-us/pricing/licensing-faq/](http://azure.microsoft.com/en-us/pricing/licensing-faq/)). So if you already have Standard OS licenses, uploading your own VHDs can save your thousands of dollars just in licensing per server. Let's get started.

#### 
Prepare your OS

There are some restrictions with what you upload into Azure 

#### 
Upload Your VHD

In the interest of having fun with Azure, let's go ahead and upload a Sysprep'd vhd into a new Azure Storage account.
# Storage Information

$storageAccountName = "contoysoweststorage2"

$containerName = "images"

# VHD Information

$sourceVHD = "J:\Server 2012R2-Standard-OWA\z_Master-Server2012R2-Standard-OWA.vhd"

$destinationVHD = "Server-2012R2-Standard-OWA.vhd" # The disk name must contain only alphanumeric characters, underscore, period, or dash.

$diskName = $destinationVHD                        # The disk name must not be longer than 256 characters. The name must not end with period or dash.

# Create a new Azure Storage Account

$storage = New-AzureStorageAccount -StorageAccountName $storageAccountName -Location "East US"

# Get Storage Key information

$key = Get-AzureStorageKey -StorageAccountName $storageAccountName

# Get Context

$context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $key.Primary

# Create the container to store blob

$container = New-AzureStorageContainer -Name $containerName -Context $context

# $container = Get-AzureStorageContainer -Name "images" -Context $context

$URI = $container.CloudBlobContainer.Uri.ToString()

$destination = $URI + "/" + $diskName

# Upload VHD

Add-AzureVhd -LocalFilePath $sourceVHD -Destination $destination -NumberOfUploaderThreads 32 -OverWrite

####
