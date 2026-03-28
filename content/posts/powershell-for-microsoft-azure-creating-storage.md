---
title: "PowerShell for Microsoft Azure- Creating Storage"
date: 2015-01-26T15:03:00+0000
categories: ["PowerShell", "Azure"]
tags: ["Container", "Storage"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

This is part 4 of the blog series on getting Started with Microsoft Azure.

[Part 1: Microsoft Azure- Getting Started](http://pcfromdc.blogspot.com/2014/12/microsoft-azure-getting-started.html)

[Part 2: PowerShell for Microsoft Azure- Getting Started](http://pcfromdc.blogspot.com/2015/01/powershell-for-microsoft-azure-getting.html)

[Part 3: Microsoft Azure- Create Geo Redundancy and Virtual Networks](http://pcfromdc.blogspot.com/2015/01/microsoft-azure-create-geo-redundancy.html)

Part 4: PowerShell for Microsoft Azure- Creating Storage (This post)

Part 5: PowerShell for Microsoft Azure- Upload VHD Images (In Progress)

Part 6: PowerShell for Microsoft Azure- Create Machines (Coming soon)

Part 7: Active Directory and DNS in the Cloud and Azure AD (Coming Soon)

-----

So what is Azure Storage? Feel free to read up on the [Introduction Azure Storage site](http://azure.microsoft.com/en-us/documentation/articles/storage-introduction/) to get a good handle on Azure storage offerings. If you are not going to read about it, please know that there is a new offering called Premium Storage, which uses SSDs for low latency IOPS to support I/O intensive systems like SharePoint and SQL. You can read up more at the [Azure Premium Storage site](http://azure.microsoft.com/en-us/documentation/articles/storage-premium-storage-preview-portal/).

Think of Azure Storage as a secure container with a unique namespace that holds all of your storage requirements. You can store all kinds of objects such as blobs, tables, and files.

#### 
Let's Get Started

Azure is a great tool for building out servers through their GUI, however, I am not a big fan of the cryptic storage containers that Azure provides if you just create a server before creating your storage. 

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgU_FH6ilQ03nV3e9E09jzqW7tZEQZDdKenvRTSh-Y8lGy-ntsr1PvQNGzMixkb9wuTucBU4EiPSNRdNoLNAEPtxZ-lI6MXBbd-Csb7uqGChyphenhyphen6lxzpIWEWXIKgExncnfVYL12mc_p7ATEc0/s1600/1-+Ugly+GUI.jpg)
This cryptic Name is carried down to the URL of the storage account.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZGf7-wK76ROTcxFNPtEO8Om6DOSgMYUllikvZTGllQSe8OFORt1ZsQGkfJs65YZrrWuE-tlkJpZLiznl4XwhRUqqbsZX-Md2-1dz5Jy10a37Z77YOLCiAP33aVsIJygPMm0E4dPKDdYNr/s1600/2-+vhd+folder.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZGf7-wK76ROTcxFNPtEO8Om6DOSgMYUllikvZTGllQSe8OFORt1ZsQGkfJs65YZrrWuE-tlkJpZLiznl4XwhRUqqbsZX-Md2-1dz5Jy10a37Z77YOLCiAP33aVsIJygPMm0E4dPKDdYNr/s1600/2-+vhd+folder.jpg)
Not a very user friendly way to go...

#### 
Creating Storage- PowerShell

In the second post of this series, [PowerShell for Microsoft Azure- Getting Started](http://pcfromdc.blogspot.com/2015/01/powershell-for-microsoft-azure-getting.html), we review just that... how to get started. Now it is time to start using some of your PowerShell skills. Let's say for example, that you wish to upload your own images into a container named, "Images" and you want to have your "Images" container live within your "Contoyso East Network". Unfortunately, storage and container names need be be all lowercase letters and/or numbers, no spaces and no dashes. By default, when you create a storage account, Geo Redundant Storage is automatically enabled. This is perfect if your storage does not require high IOPS. To create a storage account run the following:

```
$storageAccountName = "contoysoweststorage1"
$containerName = "images"
# Create a new Azure Storage Account, set GEO Replication Type to Local Redundant Storage (Standard_LRS) for heavy IOPS
$storage = New-AzureStorageAccount -StorageAccountName $storageAccountName -Location "West Europe" # -Type Standard_LRS # Local Redundant Storage
# Get Storage Key information
$key = Get-AzureStorageKey -StorageAccountName $storageAccountName
# Get Context
$context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $key.Primary
# Create the container to store blob
$container = New-AzureStorageContainer -Name $containerName -Context $context

```

This would create a Storage Account that looks like this:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7nAmHZUXo5YRXLCP22qY8BSPTvGmXHjvkNfkSYffvvriOcigFPbsUF-NctetycxL2r-kFKQSsN4bh3H5q8u9FsY6a58D9kBEeRUGlR_SvNhjp7PtFXMIXS3u1gZrXroxpdWFP1M7FDavZ/s1600/4-+West+Europe+Created.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7nAmHZUXo5YRXLCP22qY8BSPTvGmXHjvkNfkSYffvvriOcigFPbsUF-NctetycxL2r-kFKQSsN4bh3H5q8u9FsY6a58D9kBEeRUGlR_SvNhjp7PtFXMIXS3u1gZrXroxpdWFP1M7FDavZ/s1600/4-+West+Europe+Created.jpg)
And the West Europe container would look like this:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0W72NdtjnSpdp3VWVOTSqXEX470GtyUO_jE08ZvjAW63W_EIxdafScqu_ys5EPoBPCdxSjK8fWgP4HL13UVxBLro8Dpe5ixUqHeqocjNJtGvZG9JShGOX52UvLy2im1X9gKxYJZ9ezNix/s1600/5-+Container.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi0W72NdtjnSpdp3VWVOTSqXEX470GtyUO_jE08ZvjAW63W_EIxdafScqu_ys5EPoBPCdxSjK8fWgP4HL13UVxBLro8Dpe5ixUqHeqocjNJtGvZG9JShGOX52UvLy2im1X9gKxYJZ9ezNix/s1600/5-+Container.jpg)

Now, let's say that we need to put our data on SSDs as we are running SharePoint in Azure, and we want to optimize our SQL drives. Ideally you would want to put the drives in the new Premium Storage.

```
$storageAccountName = "contoysoweststorage2"
$containerName = "images"
# Create a new Azure Storage Account
New-AzureStorageAccount -StorageAccountName $storageAccountName -Location "West Europe" -Type Premium_LRS
# Get Storage Key information
$key = Get-AzureStorageKey -StorageAccountName $storageAccountName
# Get Context
$context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $key.Primary
# Create the container to store blob
$container = New-AzureStorageContainer -Name $containerName -Context $context

```

#### 
Unfortunately, at the time of this blog post, Azure Premium storage is only available in the West US, East US 2, and West Europe. If your Azure account is not set up correctly, you will receive notice that your "subscription is not authorized for feature Premium Storage". [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj0pVzMi97ZesgqCsK4OxN4mRf52e4kxpbue5hxMHHcVCU2I47eK7LKd2rwEwY1mfRGAQxbJQpilIdCrTHhe7TYAxnjiybBMDif-9D1KfPbaEZodILlrw0i0BsIg0OEk817XFV1zXqXhWwE/s1600/6-+Error.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj0pVzMi97ZesgqCsK4OxN4mRf52e4kxpbue5hxMHHcVCU2I47eK7LKd2rwEwY1mfRGAQxbJQpilIdCrTHhe7TYAxnjiybBMDif-9D1KfPbaEZodILlrw0i0BsIg0OEk817XFV1zXqXhWwE/s1600/6-+Error.jpg)

#### 
Create Storage for SQL

Just like on-premises storage for SQL, for best optimization of data transfer, you want to put your SQL storage on different LUNs. In Azure, you have the ability to break out your disk storage into different LUNs as well. 

Now, depending on the type of VM tier series that you are using (A0 Standard) will determine the number of drives that you can have attached to your VM. So while architecting your Azure infrastructure/machine, make sure the VMs that you want to use, support the number of disks required. I suggest reading through [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/en-us/library/azure/dn197896.aspx)* before building out your VMs.

Remember that in a Virtual Environment the disk associated to a specific VM is just another .vhd file that does not have an OS installed/assigned to it. So to add a disk to a SQL Server, it is easier to add the data disk after the VM has already been created. We will go through creating the data disk in Part 6: PowerShell for Azure- Creating Virtual Machines.

#### 
Warning

Do not put all of your eggs in one basket. There is a potential for a Storage Account to get corrupted, and you could lose everything. Make sure that you have backups set up to a secondary Geo-Redundant Storage Account. If you have the ability to delay the creation of your backup storage account, it will lessen the likelihood of your Storage Accounts being created in the same storage pool within Azure. Here is an example of another storage strategy:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjGl1d-5ikH7oHV8W9uAAxe6NtOJJzar1xQ1W0tZ_kZ_FRk3WuAqAQtiZtw0mVWT5PYtQom2WCI1C4X3G_lLLngiwCOAJ8fxWm9sasn89TBg8jYw9DCM27_biXrctSVbUVt-xAVfG0GvTcb/s1600/7-+Storage+Drawings.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjGl1d-5ikH7oHV8W9uAAxe6NtOJJzar1xQ1W0tZ_kZ_FRk3WuAqAQtiZtw0mVWT5PYtQom2WCI1C4X3G_lLLngiwCOAJ8fxWm9sasn89TBg8jYw9DCM27_biXrctSVbUVt-xAVfG0GvTcb/s1600/7-+Storage+Drawings.jpg)

And once again, any time you are building out anything in Azure, you will want to do it through PowerShell:

```
# Local Redundant Storage Information
$lrsAccountNames = @("contoysostorage1east","contoysostorage2east","contoysobustorage1east","contoysobustorage2east")
$lrsContainerNames = @("images","sql-drives","data-drives")
# Global Redundant Storage Information
$grsAccountNames = @("contoysosqlbus")
$grsContainerNames = @("production-bus","development-bus")
# Location Information
$location = "East US"
# Pause Time before Creating Backup Storage Account in Minutes
$time = 120
# Create Local Redundant Storage
foreach ($lrsAccountName in $lrsAccountNames) {
    # Create a new Azure Storage Account, set GEO Replication Type to Local Redundant Storage (Standard_LRS) for heavy IOPS
    $storage = New-AzureStorageAccount -StorageAccountName $lrsAccountName -Location $location -Type Standard_LRS # Local Redundant Storage
    Write-Host("$lrsAccountName storage account created...")
    # Get Storage Key information
    $key = Get-AzureStorageKey -StorageAccountName $lrsAccountName
    # Get Context
    $context = New-AzureStorageContext -StorageAccountName $lrsAccountName -StorageAccountKey $key.Primary
    foreach ($lrsContainerName in $lrsContainerNames) {
        # Create the container to store blob
        $container = New-AzureStorageContainer -Name $lrsContainerName -Context $context
        Write-Host("$lrsContainerName container created...")
    }
}
# Pause to create backup storage account$counter = 0
do {
    Start-Sleep -Seconds 60
    Write-Host("$counter of $time minutes completed...")
    $counter ++
}
While ($counter -le $time)
# Create Global Redundant Storage
foreach ($grsAccountName in $grsAccountNames) {
    # Create a new Azure Storage Account, set GEO Replication Type to Local Redundant Storage (Standard_LRS) for heavy IOPS
    $storage = New-AzureStorageAccount -StorageAccountName $grsAccountName -Location $location # -Type Standard_LRS # Local Redundant Storage
    Write-Host("$grsAccountName storage account created...")
    # Get Storage Key information
    $key = Get-AzureStorageKey -StorageAccountName $grsAccountName
    # Get Context
    $context = New-AzureStorageContext -StorageAccountName $grsAccountName -StorageAccountKey $key.Primary
    foreach ($grsContainerName in $grsContainerNames) {
        # Create the container to store blob
        $container = New-AzureStorageContainer -Name $grsContainerName -Context $context
        Write-Host("$grsContainerName container created...")
    }
}

```

#### 
If you take a look at the image of the locally redundant storage, you will notice that there are containers called "vhds". The PowerShell script does not create these containers since Azure will create the containers as needed when the Virtual Machines are created.

#### 
Conclusion

Managing your storage is very important when it comes to optimizing your IOPS and system latency. It is also very nice was to make sure that you keep your naming schema nice and clean, and using PowerShell to create your storage also helps keep everything user friendly.

Now that the storage infrastructure is in-place, it is time to upload a .vhd into the Images folder. Please see my next post on Uploading VHDs.

#### 
Updates

03/31/2015 Added the Warning section about the potential for Storage Account corruption.

40/02/2015 Added image and PowerShell to Warning section
