---
title: "Deploy and Retract .wsp Files using STSADM or PowerShell"
date: 2011-03-28T16:36:00+0000
categories: ["PowerShell", "SharePoint"]
tags: ["STSADM", "SharePoint 2007", "WSP Files"]
aliases:
  - "/2011/03/deploy-and-retract-wsp-files-using.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

As recommended in previous posts on using STSADM; I suggest creating a .cmd file to run the scripts, and save the scripts in a folder.  I am also assuming that you have STSADM added to your variable path.  Don't forget to right click, and run as Administrator!**

Deploy with STSADM:****

```
stsadm -o addsolution -filename path\solutionName.wsp
stsadm -o deploysolution -name solutionName.wsp -immediate -allowgacdeployment -force -allcontenturls
stsadm -o execadmsvcjobs
pause
iisreset /noforce

```

Retract with STSADM:****

```
stsadm -o retractsolution -name solutionName.wsp -immediate -allcontenturls
stsadm -o execadmsvcjobs
pause
stsadm -o deletesolution -name solutionName.wsp -override
pause
iisreset /noforce

```

Remember, to run the following commands you must have SPShellAdmin permissions (see Add-[SPShellAdmin](http://technet.microsoft.com/en-us/library/ff607596.aspx))

Add, Install, Enable, Update, Disable, Uninstall, and Remove Farm or User Solutions (SPUserSolution) with PowerShell:**

1) Update lines 1-4 (and line 6 if working with sandboxed solutions).

2) Remove the pound(#) symbol of the command you want to run.

```
$fileLocation = "C:\Projects\Event Receiver\bin\Debug"
$wspFileName = "Event_Receiver.wsp"
$featureIdentity = "Event Receiver_Feature1"
$url = "http://pc2010.local/"
# Sandboxed Solution Upgrade Name
$toSolutionName = "Event_Receiver_v2.wsp"

Add-PSSnapin Microsoft.SharePoint.PowerShell -EA 0
$literalPath = $fileLocation + "\" + $wspFileName
Write-Host("Using WSP from: " + $literalPath)

# Add WSP Solution (http://technet.microsoft.com/en-us/library/ff607552.aspx)
# Write-Host("Adding solution to Farm..."); Add-SPSolution -literalpath $literalPath

# Update Existing WSP Solution (http://technet.microsoft.com/en-us/library/ff607724.aspx)
# Write-Host("Updating solution in Farm..."); Update-SPSolution -identity $wspFileName -literalpath $literalPath -gacdeployment

# Deploy WSP solution to the Farm (http://technet.microsoft.com/en-us/library/ff607534.aspx)
# Write-Host("Deploying solution to the Farm..."); Install-SPSolution -identity $wspFileName -allwebapplications -gacdeployment -force

# Enable an installed feature at the given scope (http://technet.microsoft.com/en-us/library/ff607803.aspx)
# If the feature is a farm feature, no URL is needed
# Write-Host("Enabling Feature..."); Enable-SPFeature -identity $featureIdentity -url $url

# Disable a feature at the given scope (http://technet.microsoft.com/en-us/library/ff607879.aspx)
# If the feature is a farm feature, comment out -URL Parameter
# Write-Host("Disabling Feature..."); Disable-SPFeature -identity $featureIdentity -force -confirm:$false -url $url

# Retract WSP solution from the farm (http://technet.microsoft.com/en-us/library/ff607873.aspx)
# Write-Host("Retracting Solution from Farm..."); Uninstall-SPSolution -identity $wspFileName -confirm:$false

# Delete WSP solution from the farm (http://technet.microsoft.com/en-us/library/ff607748.aspx)
# Write-Host("Deleting solution from Farm..."); Remove-SPSolution -identity $wspFileName -force -confirm:$false

# Add sandboxed solution to solution gallery (http://technet.microsoft.com/en-us/library/ff607715.aspx)
# Write-Host("Adding user solution to solution gallery..."); Add-SPUserSolution -LiteralPath $literalPath -Site $url

# Activate the sandboxed solution in a site collection (http://technet.microsoft.com/en-us/library/ff607653.aspx)
# Write-Host("Enabeling sandboxed solution..."); Install-SPUserSolution -identity $wspFileName -Site $url

# Upgrade EXISTING activated sandboxed solution (http://technet.microsoft.com/en-us/library/ff607902.aspx)
# Write-Host("Updating sandboxed solution..."); Update-SPUserSolution -identity $wspFileName -Site $url -ToSolution $toSolutionName

# Deactivate a sandboxed solution from site collection (http://technet.microsoft.com/en-us/library/ff607582.aspx)
# Write-Host("Retracting sandboxed solution..."); Uninstall-SPUserSolution -identity $wspFileName -Site $url -confirm:$false

# Delete a sandboxed solution from site collection (http://technet.microsoft.com/en-us/library/ff607709.aspx)
# Write-Host("Deleting sandboxed solution..."); Remove-SPUserSolution -identity $wspFileName -Site $url -confirm:$false

```

Update (11/2/2014)

Finally added the ability to Add, Enable, Update, Deactivate, and Remove Sandboxed Solutions (SPUserSolutions).

Added write-host output so that you know what you did...

Update (01/26/2015)

Added a closing parenthesis ")" for installing solution, and fixed some spelling errors
