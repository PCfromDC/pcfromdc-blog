---
title: "SharePoint 2013 and Web Application Proxy (WAP) Server"
date: 2015-02-10T03:46:00+0000
categories: ["PowerShell", "Networking"]
tags: ["HTTP Redirect", "Miguel Wood", "HTTPS", "WAP", "HTTP", "ADFS", "Web Application Proxy Server"]
aliases:
  - "/2015/02/sharepoint-2013-and-web-application.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

The other day I was talking with a friend of mine, Miguel Wood ([@MiguelWood](https://twitter.com/miguelwood)) about a SharePoint 2013 farm that I was provisioning in Azure. We were talking about endpoints and direct access into the SharePoint farm, and I realized that I really should have put all of my public facing URLs through a reverse proxy. Luckily, I had a Windows Server 2012R2 WAP server in Azure already, handling all of the ADFS 3.0 authentication traffic, so I updated my DNS entries to point at the WAP Load Balancing URL.

This is where things started to get a bit messy. After updating the URLs and adding the SSL certificates into the WAP server, I was able to hit my public facing URLs. I was very happy to add this added level of security into the environment. My happiness was short lived because when I went and clicked on an application that lived in the App domain, I received a 404 page not found error.

Silly me, I had not set up the WAP server to pass the app domain. After spending way too much time on trying to figure it out, I decided that I had best do a bit of research. I came across the this article in TechNet, [https://technet.microsoft.com/en-us/library/dn383655.aspx](https://technet.microsoft.com/en-us/library/dn383655.aspx), that explains why I was not able to pass the app domain successfully.

[*](/images/Not Supported.png)
Well, this is a bit frustrating... So I went back into DNS and pointed the app domain back to the Cloud Service URL for the app domain. Long story, short... I reverted back to creating CNAMEs for all of the public facing URLs to the appropriate Azure Cloud Service.

#### 
But Wait There's More!

Luckily there is a new version of the Web Application Proxy coming out wit the new Windows Server OS. It is still in beta, but according to the this Application Proxy Blog post, [http://blogs.technet.com/b/applicationproxyblog/archive/2014/10/01/introducing-the-next-version-of-web-application-proxy.aspx](http://blogs.technet.com/b/applicationproxyblog/archive/2014/10/01/introducing-the-next-version-of-web-application-proxy.aspx), 

# 
Wildcard domain
publishing – new patterns and easier SharePoint 2013 apps publishing

We are adding the ability to publish not only a specific
domain name but an entire sub-domain. This opens new opportunities for customer
that want to publish sites in bulk and not one by one. For example, if all your
apps are under [http://*.apps.internal/](http://%2A.apps.internal/), you can publish them using a single external domain like [https://*.apps.contoso.com/](https://%2A.apps.contoso.com/). *

*This pattern is important when there is a need to publish
SharePoint 2013 apps that uses a special sub-domain to all apps. In this case,
only wildcard certificates would work as the specific apps domain may change
over time.*

Ok, that is all good, so lets see how to setup the new WAP server for SharePoint 2013, including the App Domain.

#### 
Let's Get Started

If you are already running WAP servers, you will run into issues, unless you remove the older versions first. Ideally you have a development environment to install ADFS and WAP, as you do not want to deploy the technical evaluation bits in your production environment. As just mentioned, if you do add the new WAP server into an older WAP cluster, you will run into this issue:

[*](/images/old wap error.png)
I recommend standing up a new ADFS server on a seperate VLAN that WAP can connect to, but not actually use the server for ADFS.

The first thing you will need to do is grab the ISO from the TechNet Evaluation Center, [http://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-technical-preview](http://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-technical-preview). 

Create a new server for ADFS and a new server for WAP

After you install the OS on the WAP server, make sure that you have at least 2 NIC ports to separate your traffic. You will use one NIC for your incoming Internet traffic and one NIC for your connection to your Internal Network. 

You will want to add the server to your network but NOT to your domain. Do NOT add the server to your DOMAIN.

This WAP server for this demonstration is going to be used solely for a reverse proxy for SharePoint, and not for ADFS. There are plenty of blog posts on how to set-up ADFS 3.0, but you will want to install ADFS off the Technical Preview bits. Here is a simple [post](http://www.office365tipoftheday.com/2014/01/07/setting-adfs-3-0-server-2012-r2-office-365/) that installs ADFS on Server 2012R2, nothing has changed.

You will need your App Domain Wildcard Certificate and your SharePoint SAN certificates with Key.

For this blog post, we will add the WAP server to the network, configure the reverse proxy for SharePoint 2013 hybrid, including the app domain on-premises, and re-point the DNS entries to the new WAP server

#### 
Prepare The Server

Ideally you would want to run your WAP server with 2 NIC cards, so at this point, label and set your 2 NIC Cards, and assign your IP Addresses for each. For example, to start, my Network looks like this: 

[![](/images/Networks.png)](/images/Networks.png)

My external facing NIC looks like this:

[![](/images/external nic.png)](/images/external nic.png)

My internal facing NIC looks like this:

[![](/images/internal nic2.png)](/images/internal nic2.png)
Adding the default gateway information the DNS information will allow us to not rely on using the HOSTS file and will update our Network Types:

[![](/images/NIC After.png)](/images/NIC After.png)
By having the NICs on different network types, you can then close off the ports appropriately.

After setting up your NICs clean up your firewall so that only the required ports each network type are set up correctly. For example, you do not want to be using Remote Desktop from the Internet side of your server. Make sure that you open up port 80 on the Public side of your firewall, but keep it closed on the internal side. 

[![](/images/Firewall Rules.png)](/images/Firewall Rules.png)

Next is to enable the WAP server feature.

```
Add-WindowsFeature -Name web-application-proxy -IncludeManagementTools
```

If we run:

```
Get-Command –Module WebApplicationProxy
```

on bother the old and new WAP server, during the beta phase at least, the available cmdlets are the same.

[![](/images/old vs new.png)](/images/old vs new.png)

#### 
Import / Export Certificates

We will need to import certificates with that contain private keys. We will be populating the WAP server with the following information:

[![](/images/Certificate Info.png)](/images/Certificate Info.png)

We have placed all of our certificates in the C:\Certificates\Imports *folder. To import them all run the following:

```
$importFolder = "C:\Certificates\Imports"
$file = (Get-ChildItem -Path $importFolder -Recurse)
$certificate = $file | Import-Certificate -CertStoreLocation  Cert:\LocalMachine\My
$certificate.DnsNameList | Select Unicode 
```

Now, if your WAP server is in production, you should have a fail-over onto another WAP server. Remember that at this time we are working with the Technical Preview bits and not for Production Use. To export your certificates to move to your next WAP server:

```
$exportFolder = "C:\Certificates\Exports"
$pw = ConvertTo-SecureString -String "Pass@w0rd#1" -Force -AsPlainText
$certs = Get-ChildItem -Path cert:\LocalMachine\my | Where-Object {$_.HasPrivateKey -eq $true}
foreach ($cert in $certs) {
    $filePath = $exportFolder + "\" + $cert.FriendlyName.ToString() + ".pfx"
    $cert | Export-PfxCertificate -FilePath $filePath -Password $pw -ChainOption BuildChain
} 
```

To import the .pfx certificates into the next WAP server:

```
$importFolder = "C:\Certificates\Imports"
$file = (Get-ChildItem -Path $importFolder -Recurse)
$pw = ConvertTo-SecureString -String "Pass@w0rd#1" -Force -AsPlainText
$certificate = $file | Import-PfxCertificate -CertStoreLocation  Cert:\LocalMachine\My -Password $pw -Exportable #If you want to export again
$certificate.DnsNameList | Select Unicode

```

Now that the certificates are installed, it's time to set-up WAP for the incoming URLs. But first let's first compare the take a look at the Add-WebApplicationProxyApplication cmdlet from Server 2012R2 to the Technical Preview.

[*](/images/cmdlet comparison.png)

The items in Yellow are the new properties at this point. Now the one that is really important to me is the EnableHTTPRedirect.* This will allow users to go to http://sp2013.contoso.com and get redirected to https://sp2013.contoso.com automatically without having to open port 80 on your Firewall (Private Network) or on your SharePoint server. Basically this stops us from having to  create a redirect in IIS as described in my blog post *[How to Redirect from HTTP to HTTPS with URL Rewrite](/posts/how-to-redirect-from-http-to-https-with/)*

#### 
Update HOSTS File

If you do not put in any DNS server information in your internal NIC settings, you will have to put all of your DNS entries in the HOSTS file. Since our WAP server is not attached to any domain, and does not have access to the internal DNS, you will need to modify the server's HOSTS file so that the redirects can be redirected correctly.

You will find the HOSTS file here(typically): *C:\Windows\System32\drivers\etc* 

Since your ADFS server should be a demo server as well, you will also want to include the ADFS server address and URL in your HOSTS file. 

#### 
Bind to ADFS

The WAP server is meant to be associated with an existing ADFS farm, and to complete the installation of the WAP server, you will need to either run the Installation Wizard or run the Install-WebApplicationProxy cmdlet. If you are not using the GUI, your PowerShell will look something like this:

```
$certFriendlyName = "sts.domain.net"
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint
$FScredential = Get-Credential
Install-WebApplicationProxy -FederationServiceName "sts.domain.net" -FederationServiceTrustCredential $FScredential -CertificateThumbprint $thumbprint

```

#### 
Publish the SharePoint On-Premises and WAC Web Applications 

The PowerShell for creating the web applications will be the same for your SharePoint needs (not for hybrid) as it will be for your WAC server.

```
$externalURL = "https://sp2013.pcdemo.net" # Incoming URL
$backendURL = "https://sp2013.pcdemo.net"  # URL for SharePoint WA
$name = "sp2013.pcdemo.net"                # WAC Web Application Name
$certFriendlyName = "san.pcDemo.net"       # Cert Friendly Name
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint
Add-WebApplicationProxyApplication -ExternalPreauthentication PassThrough `
    -ExternalUrl $externalURL `
    -BackendServerUrl $backendURL 
    -name $name `
    -ExternalCertificateThumbprint $thumbprint `
    -ClientCertificatePreauthenticationThumbprint $thumbprint `
    -DisableTranslateUrlInRequestHeaders:$False `
    -DisableTranslateUrlInResponseHeaders:$False `
    -EnableHTTPRedirect:$true
```

If you want to create Web Applications for all of the DNS names in your SAN certificate you can run this script:

```
$certFriendlyName = "san.pcDemo.net"       # Cert Friendly Name
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$dnsNames = $cert.DnsNameList | Select Unicode
foreach ($dnsName in $dnsNames) {
    $externalURL = "https://" + $dnsName.Unicode
    $backendURL = "https://" + $dnsName.Unicode
    $name = $dnsName.Unicode
    $thumbprint = $cert.Thumbprint
    Add-WebApplicationProxyApplication -ExternalPreauthentication PassThrough `
        -ExternalUrl $externalURL `
        -BackendServerUrl $backendURL `
        -name $name `
        -ExternalCertificateThumbprint $thumbprint `
        -ClientCertificatePreauthenticationThumbprint $thumbprint `
        -DisableTranslateUrlInRequestHeaders:$False `
        -DisableTranslateUrlInResponseHeaders:$False `
        -EnableHTTPRedirect:$true 
}

```

#### 
Publish the SharePoint Hybrid Web Application

This one is a bit different since the hybrid connection relies on pre-authentication using a certificate that you have uploaded to O365.

```
$externalURL = "https://hybrid.pcdemo.net"  # Incoming URL
$backendURL = "https://hybrid.pcdemo.net"   # URL for SharePoint WA
$name = "hybrid.pcdemo.net"                 # WAC Web Application Name
$certFriendlyName = "san.pcDemo.net"        # Cert Friendly Name
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint
Add-WebApplicationProxyApplication -ExternalPreauthentication ClientCertificate `
   -ExternalUrl $externalURL `
   -BackendServerUrl $backendURL `
   -name $name `
   -ExternalCertificateThumbprint $thumbprint `
   -ClientCertificatePreauthenticationThumbprint $thumbprint `
   -DisableTranslateUrlInRequestHeaders:$False `
   -DisableTranslateUrlInResponseHeaders:$False
```

#### 
Publish the SharePoint App Domain

There is an issue with not having access to the DNS of your internal netowork, and that would be that the HOSTS file does not accept wildcard entries. To get around this, you can install a tool like Acrylic DNS Proxy [http://mayakron.altervista.org/wikibase/show.php?id=AcrylicHome](http://mayakron.altervista.org/wikibase/show.php?id=AcrylicHome) 

To create your wildcard domain Web Application for your App Domain, run the following script.

```
$externalURL = "https://*.pcdemo-apps.net"  # Incoming URL
$backendURL = "https://*.pcdemo-apps.net"   # URL for SharePoint WA
$name = "*.pcdemo-apps.net"                 # WAC Web Application Name
$certFriendlyName = "pcDemo-apps.net"       # Cert Friendly Name
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint
Add-WebApplicationProxyApplication -ExternalPreauthentication PassThrough `
    -ExternalUrl $externalURL `
    -BackendServerUrl $backendURL `
    -name $name `
    -ExternalCertificateThumbprint $thumbprint `
    -ClientCertificatePreauthenticationThumbprint $thumbprint `
    -DisableTranslateUrlInRequestHeaders:$False `
    -DisableTranslateUrlInResponseHeaders:$False `
    -EnableHTTPRedirect:$true 

```

After you have created all of your Web Applications, if you go into the Remote Access Management Console, your list of Published Web Applications might look similar to this:

[![](/images/WA List.png)](/images/WA List.png)
There you go! You now have the ability to create all the WAC Web Applications that you will need to allow external users into your SharePoint environment safely. It is easy to edit or delete the Web Applications in the GUI if you make a mistake.

[![](/images/edit 1.png)](/images/edit 1.png)

They will even supply the PowerShell for later use.

[![](/images/edit 2.png)](/images/edit 2.png)

Once you have ADFS up and running, regardless if you use it or not, the new WAP server is even better than the previous product. Hopefully this post will help get you started on the road to creating a SharePoint Hybrid environment or just creating a reverse proxy server to keep your SharePoint (or any other site) safe.

#### 
Test It Out!

Click on the links below and notice that they get redirected from port 80 to port 443.

[http://sp2010.pcdemo.net ](http://sp2010.pcdemo.net/)

[http://sp2013.pcdemo.net](http://sp2013.pcdemo.net/)
