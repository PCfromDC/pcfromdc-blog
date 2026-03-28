---
title: "Copying BLOBs Between Azure Storage Containers"
date: 2015-07-20T12:30:00+0000
categories: ["PowerShell", "Azure"]
tags: ["Azure for Government", "BLOB", "Azure Containers", "Copy", "AzureStorageBlobCopy", "Government Cloud", "Azure Storage"]
aliases:
  - "/2015/07/copying-blobs-between-azure-storage.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

In the past when I needed to move BLOBs between Azure Containers I would use a script that I put together based off of [Michael Washam's](https://twitter.com/mwashamtx) blog post [Copying VHDs (Blobs) between Storage Accounts](http://michaelwasham.com/windows-azure-powershell-reference-guide/copying-vhds-blobs-between-storage-accounts/). However with my latest project, I actually needed to move several BLOBs from the Azure Commercial Cloud to the Azure Government Cloud.

Right off the bat, the first problem is that the endpoint for the Government Cloud is not the default endpoint when using PowerShell cmdlets. So after spending some time updating my script to work in Commercial or Government Azure, I was still not able to move anything. So after a bit of "this worked before, why are you not working now?" frustration, it was time for plan B.

#### 
Plan B

Luckily enough the Windows Azure Storage Team had put together a command line utility called AzCopy. AzCopy is a very powerful tool as it will allow you to copy items from a machine on your local network into Azure. It will also copy items from one Azure tenant to another Azure tenant. The problem that I ran into is that the copy is synchronous, meaning that it copies one item at a time, and you cannot start another copy until the previous operation has finished. I also ran the command line in ISE vs directly in the command line, which was not as nice. In the AzCopy command line utility, a status is displayed letting you know elapsed time and when the copy has completed. In ISE, you know your BLOB is copied when script has completed running. You can read up on and download AzCopy from [Getting Started with the AzCopy Command-Line Utility](https://azure.microsoft.com/en-us/documentation/articles/storage-use-azcopy/). This is the script that I used to move BLOBs between the Azure Commercial Tenant and the Azure Government Tenant.

```
$sourceContainer = "https://commercialsharepoint.blob.core.windows.net/images"
$sourceKey = "insert your key here"
$destinationContainer = "https://governmentsharepoint.blob.core.usgovcloudapi.net/images"
$destinationKey = "insert your key here"
$file1 = "Server2012R2-Standard-OWA.vhd"
$file2 = "Server2012R2-Standard-SP2013.vhd"
$file3 = "Server2012R2-Standard-SQL2014-Enterprise.vhd"
$files = @($file1,$file2,$file3)
function copyFiles {
    foreach ($file in $files) {
        & 'C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe' /Source:$sourceContainer /Dest:$destinationContainer /SourceKey:$sourceKey /DestKey:$destinationKey /Pattern:$file 
    }
}
function copyAllFiles {
    & 'C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe' /Source:$sourceContainer /Dest:$destinationContainer /SourceKey:$sourceKey /DestKey:$destinationKey /S
}
# copyFiles
# copyAllFiles

```

While I was waiting for my BLOBs to copy over, I decided to look back at my Plan A,and see if I could figure out my issue(s).

#### 
Plan A

After cleaning up my script and taking a bit of a "Type-A personality" look at the script, I noticed that i was grabbing the Azure Container Object, but not grabbing the BLOB Object before copying the item. Once I piped the container to the BLOB before copying, it all worked as expected. Below is my script, but please notice that on the Start-AzureStorageBlobCopy cmdlet, I am using the -Force parameter to overwrite the existing destination BLOB if it exists.

```
# Source Storage Information
$srcStorageAccount = "commercialsharepoint"
$srcContainer = "images"
$srcStorageKey = "insert your key here"
$srcEndpoint = "core.windows.net"
# Destination Storage Information
$destStorageAccount  = "governmentsharepoint"  
$destContainer = "images"
$destStorageKey = "insert your key here"
$destEndpoint = "core.usgovcloudapi.net" 
# Individual File Names (if required)
$file1 = "Server2012R2-Standard-OWA.vhd"
$file2 = "Server2012R2-Standard-SP2013.vhd"
$file3 = "Server2012R2-Standard-SQL2014-Enterprise.vhd"
# Create file name array
$files = @($file1, $file2, $file3)
# Create blobs array
$blobStatus = @()
### Create the source storage account context ### 
$srcContext = New-AzureStorageContext   -StorageAccountName $srcStorageAccount `
                                        -StorageAccountKey $srcStorageKey `
                                        -Endpoint $srcEndpoint 
### Create the destination storage account context ### 
$destContext = New-AzureStorageContext  -StorageAccountName $destStorageAccount `
                                        -StorageAccountKey $destStorageKey `
                                        -Endpoint $destEndpoint
#region Copy Specific Files in Container
    function copyFiles {
        $i = 0
        foreach ($file in $files) {
            $files[$i] = Get-AzureStorageContainer -Name $srcContainer -Context $srcContext | 
                         Get-AzureStorageBlob -Blob $file | 
                         Start-AzureStorageBlobCopy -DestContainer $destContainer -DestContext $destContext -DestBlob $file -ConcurrentTaskCount 512 -Force 
            $i++ 
        }  
        getBlobStatus -blobs $files     
    }
#endregion
#region Copy All Files in Container
    function copyAllFiles {
        $destBlobName = $blob.Name
        $blobs = Get-AzureStorageContainer -Name $srcContainer -Context $srcContext | Get-AzureStorageBlob
        $i = 0
        foreach ($blob in $blobs) {
            $blobs[$i] =  Get-AzureStorageContainer -Name $srcContainer -Context $srcContext | 
                          Get-AzureStorageBlob -Blob $blob.Name | 
                          Start-AzureStorageBlobCopy -DestContainer $destContainer -DestContext $destContext -DestBlob $destBlobName -ConcurrentTaskCount 512 -Force
            $i ++
        }  
        getBlobStatus -blobs $blobs 
    }
#endregion
#region Get Blob Copy Status
    function getBlobStatus($blobs) {
        $completed = $false
        While($completed -ne $true){
            foreach ($blob in $blobs) {
                $counter = 0
                $status = $blob | Get-AzureStorageBlobCopyState
                Write-Host($blob.Name + " has a status of: "+ $status.status) 
                if ($status.status -ne "Success") {
                    $counter ++
                }
                if ($counter -eq 0) {
                    $completed = $true
                }   
                ELSE {
                    Write-Host("Waiting 30 seconds...")
                    Start-Sleep -Seconds 30
                }                         
            }
        }
    }        
#endregion
# copyFiles
# copyAllFiles

```

#### 
Conclusion

Having more than one way to get something accomplished within Azure if fantastic. There is not a lot of documentation out there on how to work with Azure and PowerShell within the Government Cloud, so hopefully this will make life easier for someone. Remember that these scripts can be used across any tenant, Commercial and Government and On-Premises.

#### 
Updates

08/05/2015 Fixed cut and paste variable issues and added $destBlobName for renaming BLOBs at the destination location, and updated BLOB status check wait time.
