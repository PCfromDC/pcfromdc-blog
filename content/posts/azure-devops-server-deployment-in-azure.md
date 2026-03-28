---
title: "Azure DevOps Server Deployment... In Azure!"
date: 2019-11-02T18:28:00+0000
draft: true
aliases:
  - "/2019/11/azure-devops-server-deployment-in-azure.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

We are going to look at how to create a stand-alone Azure DevOps Server, using Azure SQL Databases and Azure Analysis Service.

You will want to download Azure DevOps Server with Update 1:

[https://devblogs.microsoft.com/devops/now-available-azure-devops-server-2019-update-1-rtw/](https://download.visualstudio.microsoft.com/download/pr/4729ddf1-8892-4103-ae1f-3da43c85f203/bb4b28a6d3e83015d682526ed83246d5/azuredevopsserver2019.1.exe)

This installs Product Version 17.153.29207.5 ([INSTALL_DIR]\Application Tier\Web Services\bin\Microsoft.TeamFoundation.Framework.Server.dll.) This will fail... and grey out the ability to do advanced installation

You can find the latest information about Patches and AzDO at: [https://devblogs.microsoft.com/devops/](https://devblogs.microsoft.com/devops/)

I would suggest installing the latest server version and latest patch released.

If you are looking for a scripted installation of the bits:

I am not going to get into the step by steps on how to set up the database side, as you can follow the instructions here:

[https://docs.microsoft.com/en-us/azure/devops/server/install/install-azure-sql?view=azure-devops](https://docs.microsoft.com/en-us/azure/devops/server/install/install-azure-sql?view=azure-devops)

Make sure you have all of the Services created in Azure and make sure to create BOTH databases with the EXACT names, "AzureDevOps_Configuration"  and "AzureDevOps_DefaultCollection" prior to running the configuration wizard.

Here is an updated .sql script to add the VM Managed Instance User Role:

Here is an updated .sql script to run on each db to add the VM Managed Instance as a user on each db:

patch 3 info

https://devblogs.microsoft.com/devops/september-patches-for-azure-devops-server-and-team-foundation-server/

I have not had a chance to start testing again, but I did
find what’s going on from a deployment perspective…

I was deploying the one on the left ([https://go.microsoft.com/fwlink/?LinkId=2089023](https://go.microsoft.com/fwlink/?LinkId=2089023))
, with patch 3 ([https://aka.ms/azdev2019.0.1patch](https://aka.ms/azdev2019.0.1patch))

Notice that the file names are different with new major
version release…

    $path = "C:\Users\az_admin\Downloads\devops2019.0.1patch3.exe"

    Start-Process $path -ArgumentList "Patch -force -nowait" # "/Full /Passive" -Wait

& "C:\Program Files\Azure DevOps Server 2019\Search\zip\Configure-TFSSearch.ps1" remove

advanced and azure installs require domain joined machines, basic does not have the ability to enable deployment to pre-created blank databases in azure

Create svc_azdoService and svc_azureDataGateway service accounts

Start-Process -filePath "F:\AzureDevOpsServer2019.1.exe" -ArgumentList "/passive /full" -Wait

New-NetFirewallRule -DisplayName "Open for Data Gateway" -LocalPort 443,5671,5672,9350-9354 -Direction Outbound -Protocol TCP -Action Allow

Install data gateway first to get the region... then install analytics, servers... The October 2019 install did not allow to change regions

Start-Process -filePath "C:\Downloads\azuredevopsserver2019.0.1.exe" -ArgumentList "/passive /full" -Wait

Start-Process -filePath "C:\Downloads\devops2019.0.1patch3.exe" -ArgumentList "Patch -force -nowait" -Wait

Start-Process -filePath "F:\AzureDevOpsServer2019.1.exe" -ArgumentList "/passive /full" -Wait

[https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-gateway-install](https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-gateway-install)

Once you close the Configuration Wizard, the ability to add reporting goes away...

Analytics:

[https://docs.microsoft.com/en-us/azure/devops/report/dashboards/analytics-extension?view=azure-devops-2019#install-analytics](https://docs.microsoft.com/en-us/azure/devops/report/dashboards/analytics-extension?view=azure-devops-2019#install-analytics)

[https://docs.microsoft.com/en-us/azure/devops/report/dashboards/analytics-extension?view=azure-devops-2019#install-analytics](https://docs.microsoft.com/en-us/azure/devops/report/dashboards/analytics-extension?view=azure-devops-2019#install-analytics)

[https://docs.microsoft.com/en-us/azure/devops/report/dashboards/analytics-extension?view=azure-devops-2019#enable-analytics](https://docs.microsoft.com/en-us/azure/devops/report/dashboards/analytics-extension?view=azure-devops-2019#enable-analytics)
