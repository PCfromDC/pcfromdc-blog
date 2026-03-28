---
title: "Provisioning SQL Server Always-On Without Rights"
date: 2015-07-03T00:37:00+0000
categories: ["PowerShell", "SQL"]
tags: ["SQL 2012", "Always-On", "Group Listeners", "HADRON", "Computer Object", "Active Directory"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Separation of roles, duties, and responsibilities in a larger corporate/government environment is a good thing. It is a good thing unless you are actually trying to get something accomplished quickly on your own. But this is why there is a separation of roles, so that one person cannot simply go and add objects into Active Directory on a whim, or play with the F5 because they watched a video on YouTube.
I recently had designed a solution that was going to take advantage of SQL Server 2012 High Availability and Disaster Recovery Always-On Group Listeners. The problem was that I was not a domain admin, and did not have rights to create a computer object for the Server OS Windows Cluster, or the SQL Group Listener.

#### 
Creating the OS Cluster

Creating the OS Cluster was the easy part, I just needed to find an administrator that had the rights to create a computer object in the domain. Once that was accomplished, I made sure that the user had local admin rights on all of the soon-to-be clustered machines, and had them run the following script:

```
$node1 = "Node-01.contoso.local"
$node2 = "Node-02.contoso.local"
$osClusternName = "THESPSQLCLUSTER"
$osClusterIP = "192.168.1.11"
# $ignoreAddress = "172.20.0.0/21"
$nodes = ($node1, $node2)
Import-Module FailoverClusters
function testCluster {
    # Test Cluster
    $test = Test-Cluster -Node (foreach{$nodes})
    $testPath = $env:HOMEPATH + "\AppData\Local\Temp\" + $test.Name.ToString()
    # View Report
    $IE=new-object -com internetexplorer.application
    $IE.navigate2($testPath)
    $IE.visible=$true
}
function buildCluster {
    # Build Cluster
    $new = New-Cluster -Name $osClusternName -Node (foreach{$nodes}) -StaticAddress $osClusterIP -NoStorage # -IgnoreNetwork $ignoreAddress
    Get-Cluster | Select *
    # View Report
    $newPath = "C:\Windows\cluster\Reports\" + $new.Name.ToString()
    $IE=new-object -com internetexplorer.application
    $IE.navigate2($newPath)
    $IE.visible=$true
}
# un-comment what you what to do...
# testCluster
buildCluster
```

#### 
Creating the Group Listener

Creating the Group Listener was a bit more challenging, but not too bad. Once the OS Cluster computer object was created (thespsqlcluster.contoso.local), the newly created computer object needed to be given rights as well.
- The cluster identity 'thespsqlcluster' needs Create
Computer Objects permissions. By default all computer objects are created in
the same container as the cluster identity 'thespsqlcluster'.
- If there is an existing computer object, verify the
Cluster Identity 'thespsqlcluster' has 'Full Control' permission to that
computer object using the Active Directory Users and Computers tool.
You will also want to make sure that the quota for computer objects for 'thespsqlcluster' has not been reached.
The domain administrator was also given Sysadmin rights to all of the SQL Server instances in the cluster.
After all the permissions were set, the Domain admin could run the following script on the Primary SQL Instance to create the Group Listener:

```
Import-Module ServerManager -EA 0
Import-Module SQLPS -DisableNameChecking -EA 0
$listenerName = "LSN-TheSPDatabases"
$server = $env:COMPUTERNAME
$path = "SQLSERVER:\sql\$server\default\availabilitygroups\"
$groups = Get-ChildItem -Path $path
$groupPath = $path + $groups[0].Name
$groupPath
New-SqlAvailabilityGroupListener `
    -Name $listenerName `
    -StaticIp "192.168.1.12/255.255.255.0" `
    -Port "1433" `
    -Path $groupPath 
```

#### 
Important

After the group listener is created, all the rights that were put in place can once again be removed with the understanding that if you wish to add another listener at another time, the permissions will have to be reinstated temporarily once again. In my case, once all of the computer objects were created successfully, all rights were removed off the cluster computer object and the domain administrator was removed from SQL.

#### 
Updates

07/06/2015 Cleaned up diction and grammar, added the Important section.

10/21/2015 Updated computer object permission requirements
