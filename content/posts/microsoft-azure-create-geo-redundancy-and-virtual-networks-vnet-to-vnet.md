---
title: "Microsoft Azure- Create Geo Redundancy and Virtual Networks (VNet to VNet)"
date: 2015-01-17T05:03:00+0000
categories: ["PowerShell", "Azure"]
tags: ["Redundancy", "configuration", "Geo Redundancy", "Connection", "Local Networks", "vNET", "Virtual Networks", "S2S", "PowerShell v5", "VNet to VNet", "Infrastructure"]
aliases:
  - "/2015/01/microsoft-azure-create-geo-redundancy.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

This is part 3 of the blog series on getting Started with Microsoft Azure.

Part 1: [Microsoft Azure- Getting Started](/posts/microsoft-azure-getting-started/)

Part 2: [PowerShell for Microsoft Azure- Getting Started](/posts/powershell-for-microsoft-azure-getting/)

Part 3: Microsoft Azure- Create Geo Redundancy and Virtual Networks (This post)

Part 4: [PowerShell for Microsoft Azure- Creating Storage](/posts/powershell-for-microsoft-azure-creating/)

Part 5: PowerShell for Microsoft Azure- Upload VHD Images (In Progress)

Part 6: PowerShell for Microsoft Azure- Create Machines (Coming soon)

Part 7: Active Directory and DNS in the Cloud and Azure AD (Coming Soon)

----------
At this point we have our Azure Tenant created, and the subscription named appropriately. We also have an idea of what data centers will be hosting Contoyso's data. If you have not looked at the first blog post, this is where we are for our data hosting.

[![](/images/4- Azure Infrastructure.png)](/images/4- Azure Infrastructure.png)

#### 
Let's Get Started

To say that Microsoft's data centers are huge, is an understatement, as the data centers are built to host, essentially, shipping containers of racked storage of servers and hardware. The biggest problem moving forward, just like any on-premises data center, is how to minimize latency between servers. To keep servers as close together to reduce latency and increase performance, Azure uses a feature called Affinity Groups. Affinity Groups aggregates the compute and storage services not just to the same data center, but into the same server cluster and was assigned to specific physical pieces of hardware. 

In the beginning, you had to tie your network to an Affinity Group, but this has caused problems within Microsoft, as the Affinity Groups were tied to hardware, and when the hardware needed to be replaced, issues arose. So, Affinity Groups are no longer required to be tied to networks. You can still associate them, but we will not be going over that in this blog post. Also, if you have an existing Affinity Group associated with a network, it will eventually be migrated to a Regional vNET. For more information read [About Regional VNets and Affinity Groups for Virtual Network](http://msdn.microsoft.com/en-us/library/azure/jj156085.aspx).

#### 
Getting Your Local VPN  IP Gateway Address

Your network administrator should be able to give this to you, or if you wish, just Bing it... "What is my ip address" to get your external gateway address. If this is a demo environment and you do not have a fixed IP, this could be a problem because you could drop your Server to Server (S2S) VPN tunnel when you glean a new IP from your provider. If this happens, all you would need to do is update Azure with your new IP address and reconnect... If this is for your production environment and you have a dynamic IP address, you should really get a static IP. 

DOCUMENT YOUR GATEWAY IP ADDRESS

#### 
Creating Your Local Networks within Azure

Now is a good time to bring back out Excel and continue with the documentation of the on-premises and Azure infrastructure.

[![](/images/8- Network Info.png)](/images/8- Network Info.png)

You can download the Excel file from [http://1drv.ms/1xvEfcW](http://1drv.ms/1xvEfcW)

The first thing that needs to be accomplished is creating the Local Networks. From the left hand navigation, select  NETWORKS, and then select LOCAL NETWORKS from the top navigation.

Once you are on the LocalNetworks page, click ADD A LOCAL NETWORK. 

[![](/images/9- Local Networks.png)](/images/9- Local Networks.png)

This will open a modal window to fill out. Hopefully you have already filled out your table and have all of the required information handy. For you first network, enter you on-premises information, with the VPN Device IP being your IP Address for your corporate gateway.

[![](/images/10- Contoyso DC.png)](/images/10- Contoyso DC.png)

On the next page, enter your IP Address and CIDR information, and click the check to continue.

[![](/images/11- Contoyso IP.png)](/images/11- Contoyso IP.png)

To add the next local network, click the NEW button at the bottom of the page and select ADD LOCAL NETWORK.

[![](/images/12- Add local network.png)](/images/12- Add local network.png)

Create your other two local networks, and when you have completed your local networks, you should have a list of local networks similar to this:

[![](/images/13- local networks.png)](/images/13- local networks.png)At this point, the virtual network IP ranges have been created, but not all the IP Addresses have been added to the VPN Gateway Addresses for East US and West Europe. Before we can add our VPN Gateway information, the virtual networks will need to be created. The gateways IP addresses will be assigned with the creation of the virtual network.

### 
Creating Virtual Networks within Azure

You can read Microsoft's marketing about virtual networks by going to  [http://azure.microsoft.com/en-us/services/virtual-network/](http://azure.microsoft.com/en-us/services/virtual-network/). But essentially, an Azure Virtual Network is the infrastructure to build out your cloud network environment within or to expand your on-premises network upon. It is your Infrastructure-as-a-Service (IaaS) and Platform-as-a-Service (PaaS) all rolled into one.

#### 
Create Your First Virtual Network

After creating the Local Networks within your Azure Tenant, it is time to create the Virtual Networks. Virtual networks are the networks within the Azure Data Centers. Since the on-premises network is already up and running, only networks that need to be created at this point are the East US and West Europe virtual networks. To get started, select the NETWORKS link in the left navigation followed by the VIRTUAL NETWORKS link in the upper navigation. This brings you to the virtualnetworks page.

[![](/images/14- Virtual Networks.png)](/images/14- Virtual Networks.png)

Click on the CREATE A VIRTUAL NETWORK link to get started. This will open up a modal window, and fill out the information appropriately (look at the table you created earlier).

[![](/images/15- Azure East 1.png)](/images/15- Azure East 1.png)

On the second page, select the Configure site-to-site VPN and select the on-premises local network. Since Contoyso is planning on extending the network into the cloud, Azure will need to know about the local DNS servers.

In Azure, DNS entries are set in a priority ranking, not in a round-robin. The first entry is the first DNS server queried, so plan appropriately. Later on we will be adding the DNS server information via the NetworkConfig.xml file as well.

[![](/images/15- Azure East 2a.png)](/images/15- Azure East 2a.png)

On the third page, click the add gateway subnet button.

[![](/images/15- Azure East 3.png)](/images/15- Azure East 3.png)

Click the check mark to finish.

After the first network has been completed, your virtualNetworks page should look like this:

[![](/images/16- East Network.png)](/images/16- East Network.png)

At this point, we need to create our gateway and get the IP address before we create any of our other networks. To do this, click on the appropriate network, in this case the Azure East US network, and select DASHBOARD from the top navigation.

At this point, since a gateway has not been created, click the CREATE GATEWAY button at the bottom of the page.

[![](/images/17- East Dashboard1.png)](/images/17- East Dashboard1.png)

This will bring up the option for either Dynamic or Static Routing... Select Dynamic routing as static routing is not supported for the Site-to-Site (S2S) VPN tunnel that is about to be created. Select Yes to create the gateway... You will notice that the gateway is now yellow, as it is now (being) created within Azure. The gateway will turn green when the S2S tunnel is not broken.

Gateway creation takes a fair amount of time to create, so go make a pot of coffee and come back in about 30 minutes. When the gateway has been created, add the gateway IP address to the Excel file.

Your dashboard should look like this:

[![](/images/18- East Dashboard2.png)](/images/18- East Dashboard2.png)

and your Excel table should look like this:

[![](/images/19- Excel Table.png)](/images/19- Excel Table.png)

After the gateway has been created, go ahead and click the CONNECT button at the bottom of the page. At this point, we have not created the RRAS connection on-premises so nothing will connect, but the connection has been activated within Azure for the time we get RRAS up and running. The final step is to add the IP address to the vNet-East settings on the localNetworks page, by clicking on LOCAL NETWORKS in the top navigation, selecting the vNet-East network, and clicking the EDIT button.

Enter your IP address on the first page:

[![](/images/20- Add Local IP-1.png)](/images/20- Add Local IP-1.png)

On the second page, click the check to finish adding the IP address.

#### 
Create Your Second Virtual Network

It is now time to create the second virtual network. From the virtualNetworks page, click NEW to start the process of creating the second virtual network (West Europe).

[![](/images/21- 2nd Network-1.png)](/images/21- 2nd Network-1.png)

On the first page, add the appropriate name from your Excel table, and appropriate location of the virtual network.

[![](/images/22- 2nd Network-2.png)](/images/22- 2nd Network-2.png)

On the second page, select the Configure a site-to-site VPN, which will add a couple of pieces of information to fill out. For this blog post, we will not be using ExpressRoute, and from the LOCAL NETWORK drop down, select the network that was just created (vNet-East). Don't forget to add the on-premises DNS server information...

[![](/images/23- 2nd Network-3.png)](/images/23- 2nd Network-3.png)

The final page will require you to create a gateway subnet, by clicking the add gateway subnet button before you finish creating your new virtual network.

[![](/images/24- 2nd Network-4.png)](/images/24- 2nd Network-4.png)

And, just as before, it is time to create the gateway and enable the connection.

Click the new virtual network name, click on DASHBOARD from the top navigation, click the CREATE GATEWAY on the bottom of the page, and select Dynamic Routing. Then select Yes to create the gateway, and once again, find something else to do for the next 20-30 minutes...

After everything is created, click the CONNECT link at the bottom of the page. At this time the dashboard page looks like this: 

[![](/images/25- Dashboard.png)](/images/25- Dashboard.png)

Grab the gateway IP address and add it to your Excel Table. Your Excel table should now be complete.

[![](/images/26- Final Table.png)](/images/26- Final Table.png)

Now that all the gateway addresses have been created, update the  LOCAL NETWORK VPN Gateway Addresses.

In the big scheme of things this is where we are:

[![](/images/27- Network Image.png)](/images/27- Network Image.png)

#### 
Add the Local Networks VPN Gateway Addresses

Click on the LOCAL NETWORKS link at the top of the Networks page. Select the East Network and click EDIT to add the East Network's Gateway Address. 

[![](/images/20- Add Local IP-1.png)](/images/20- Add Local IP-1.png)

On the second page, click the check box to continue. Then, following the same procedures, add the Gateway IP address for the West Network. When you are finished with adding the Gateway IP information, your localNetworks page should look similar to this:

[![](/images/28- Local Networks Final.png)](/images/28- Local Networks Final.png)

#### 
Connect the Networks

Now, this is where things get a bit not so fun. At this time, it is only possible to create one connection per gateway endpoint through the Azure GUI. So to get around the limitation, we will export a copy of the network as an XML file, modify it, and upload the modified file back into Azure to complete all of our network connections.

To start, go to the NETWORKS page and at the bottom of the page, click the EXPORT link. 

[![](/images/29- Exports Link.png)](/images/29- Exports Link.png)

This will open a modal window that will ask for the subscription you wish to export (see my blog post: [Microsoft Azure- Getting Started](/posts/microsoft-azure-getting-started/) to create a friendly subscription name). Once you click the check box to continue, a download dialog box will open. Save the NetworkConfig.xml document someplace handy then make a copy of it... just in case you blow up your network.

You can also download the file via PowerShell.

```
Get-AzureVNetConfig -ExportToFile c:\scripts\vNetConfig.xml
```

Remember what networks connect to what? The East network connects to DC and West networks, while the West network connects to DC and East networks.

[![](/images/30- Connections.png)](/images/30- Connections.png)

At this point we are going to crack open Visual Studio. Remember that we installed it in the last blog post, along with the Azure module for PowerShell. We reviewed the procedures to get both applications in the previous blog post,  [PowerShell for Microsoft Azure- Getting Started](/posts/powershell-for-microsoft-azure-getting/).

Open the the NetworkConfig.xml in your favorite XML editor or Visual Studio.

Within the VirtualNetworkSites schema, we are interested in modifying the ConnectionsToLocalNetwork sections by adding the appropriate reference to the LocalNetworkSiteRef.

[![](/images/31- Collapsed Config- 1.png)](/images/31- Collapsed Config- 1.png)

At this point, it is time to add the other Local Network to the gateway... Making sure that East connects to West and DC while West connects to East and DC.

[![](/images/31- Collapsed Config- 2.png)](/images/31- Collapsed Config- 2.png)

While you have the XML file open, let's look at the DNS entries as well. As discusses earlier, that if you wish to have the Azure servers know about the on-premises servers, Azure will need to know about your on-premises DNS. First, look for the DNS server(s), and them make sure the the server names and IP addresses are correct.

[![](/images/38- DNS Entry.png)](/images/38- DNS Entry.png)

At the top of the XML file, you can see that AD-02 (DNS server) is available within the VirtualNetworkConfiguration. Next, make sure that the DNS server(s) reference is associated with each network. For example, make sure that each virtual network has the AD-02 reference. If at a later time, you need to add more or edit the DNS servers, you will need to modify the XML. 

[![](/images/39- DNS Reference.png)](/images/39- DNS Reference.png)

Make sure you save your file, before you try import it back into Azure. 

Now that the DNS entries have been verified, and the network is configured correctly, it is time to upload the file back into Azure. To upload the file using PowerShell, 

```
Set-AzureVNetConfig -ConfigurationPath c:\scripts\vNetConfig.xml
```

To use the GUI, click the new button at the bottom of the page and select NETWORK SERVICES --> VIRTUAL NETWORK --> IMPORT CONFIGURATION

[![](/images/32- Import Config.png)](/images/32- Import Config.png)

and import your NetworkConfig.xml file. On the next page, Azure will show you the changes that will be happening to your network so that you can validate the changes.

[![](/images/33- Create Page 1.png)](/images/33- Create Page 1.png)

This page shows what is to be created after the original networks get deleted.

[![](/images/33- Create Page 2.png)](/images/33- Create Page 2.png)

Above, are the original networks that need to be deleted, before the networks can be recreated.

After the new networks have been created, click into the Dashboard page of one of the Networks and see that Gateway connections are still disconnected. This is because the Gateways have private keys, and the keys need to be exchanged to start the connections. To set the keys, you need to open PowerShell and log-in to your tenant. If you do not know how to use PowerShell with Azure, please review my previous blog post [PowerShell for Microsoft Azure- Getting Started](/posts/powershell-for-microsoft-azure-getting/).

We will need to create two sets of matching keys for the East and West Virtual Networks to the appropriate local network gateways. 

```
$virtualNetworkEast = "Azure East US"
$virtualNetworkWest = "Azure West Europe"
$localNetworkEast = "vNet-East"
$localNetworkWest = "vNet-West"
$localNetworkOnPrem = "Contoyso-DC"
# Set key for East to West
Set-AzureVNetGatewayKey -VNetName  $virtualNetworkEast -LocalNetworkSiteName $localNetworkWest -SharedKey "lxJeyvLdcgPutInYourOwnKeys1Aaksw1SplbnyK7YH"
# Set key for East to On-Premises
Set-AzureVNetGatewayKey -VNetName $virtualNetworkEast -LocalNetworkSiteName $localNetworkOnPrem -SharedKey "lxJeyvLdcgPutInYourOwnKeys1Aaksw1SplbnyK7YH"
# Set key for West to East
Set-AzureVNetGatewayKey -VNetName $virtualNetworkWest -LocalNetworkSiteName $localNetworkEast  -SharedKey "lxJeyvLdcgPutInYourOwnKeys1Aaksw1SplbnyK7YH"
# Set key for West to On-Premises
Set-AzureVNetGatewayKey -VNetName $virtualNetworkWest -LocalNetworkSiteName $localNetworkOnPrem -SharedKey "lxJeyvLdcgPutInYourOwnKeys1Aaksw1SplbnyK7YH"

```

#### 
Create Your Server to Server VPN Tunnel

After a couple of minutes your Azure East and West local networks will have connected. Now the final, and probably the easiest, thing to accomplish is to get the tunnel established from on-premises to your East and West Gateways.

Luckily for us, Azure has made that very simple for us. If you go to the Dashboard of your East Network, on the right hand side is a link labeled Download VPN Device Script.

[![](/images/4- VPN Download.png)](/images/4- VPN Download.png)

When you click the Download link, a modal window will pop up to select the correct type of configuration script that will need to be run on-premises. For this post we will be using Microsoft's RRAS on Server 2012R2.

[![](/images/34- Download script.png)](/images/34- Download script.png)

Clicking the check box will pop up an Open or Save dialog window. From the Save menu, select Save as, and save the file to the c:\scripts folder. After the download has been completed, rename the file to East-S2S-VPN.ps1

Once again, please open up PowerShell ISE as administrator, and open the newly downloaded file East-S2S-VPN.ps1

If you run the file as is, the required roles and features will automatically be installed. The only thing that I am not happy about with this script is that the RRAS connection name uses the IP address, not the Network name for identification. So at this point, we will modify the script to make the connection name match the Network Name.

At the bottom of line 70, is the section to install the S2S VPN connection. Within this section of code, we will be replacing the name of your gateway IP address with the name of Network. Do not do a find and replace all, as we still need the IP address to connect to the gateway.

- In the Add-VpnS2SInterface replace the -Name parameter value with the name of your Network.
- In the Set-VpnS2Sinterface replace the -Name parameter value with the name of your Network.
- In both Set-PrivateProfileString locations replace the IP Address with the name of your Network.
- In the Connect-VpnS2SInterface replace the -Name parameter value with the name of your Network.

[![](/images/35- Azure East Connection.png)](/images/35- Azure East Connection.png)

You will want to run the entire script on your RRAS server. I have never been able to run the entire script at once successfully. I suggest running the top 69 lines to install the RRAS roles and features first then running the configuration lines of the script one at a time.

After running the script on the RRAS server, go to you Routing and Remote Access Manager, to see if the VPN has connected yet. It could take several minutes to connect, and don't forget to refresh the Network Interfaces page or you could be waiting a lot longer. Your Network Interfaces page should look similar to this:

[![](/images/35- Azure East Connection- 2.png)](/images/35- Azure East Connection- 2.png)

Repeat the same process for the West network. 

After the West network has been created your Network Interfaces page should look similar to this:

[![](/images/35- Azure East Connection- 3.png)](/images/35- Azure East Connection- 3.png)

Now, if you go back into your Azure Networks page and go to the DASHBOARD page of your East Network, it should look similar to this:

[![](/images/36- east done.png)](/images/36- east done.png)

While still on your DASHBOARD page, select the West Network, and it too should say connected with a couple of green checks:

[![](/images/36- west done.png)](/images/36- west done.png)

#### 
Conclusion

At this point, you should now be able to have access to an East and a West Azure data center from your on-premises network. We have completed our goal to create a network that looks like this:

[![](/images/37- Done.png)](/images/37- Done.png)

#### 
What's Next?

The next goal is to add Active Directory and DNS into the cloud. There is a bit of work that needs to be done to get there, such as setting up storage and cloud services. So stay tuned for the next blog post PowerShell for Microsoft Azure- Creating Storage (in progress)

-----
This is part 3 of the blog series on getting Started with Microsoft Azure.

Part 1: [Microsoft Azure- Getting Started](/posts/microsoft-azure-getting-started/)

Part 2: [PowerShell for Microsoft Azure- Getting Started](/posts/powershell-for-microsoft-azure-getting/)

Part 3: Microsoft Azure- Create Geo Redundancy and Virtual Networks (This post)

UPDATES

01/19/2015 Removed section on creating Affinity Groups, and added information on why they are not needed any longer. Also added the information for inputting the on-premises DNS information.
