---
title: "Binding SSL Certificates to SNI Enabled Host Headers with PowerShell"
date: 2015-04-17T02:56:00+0000
categories: ["PowerShell", "IIS"]
tags: ["Web Binding", "HNSC", "Host Named Site Collection", "Host Headers", "SSL", "SNI"]
aliases:
  - "/2015/04/binding-ssl-certificates-to-sni.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

It is funny how some things are just easier to use a GUI to deploy versus trying to figure out a problem in code or PowerShell. That is until you run into a client that says you cannot use a GUI within their environment. With SharePoint 2013, having a single Web Application (IIS site) in IIS for Host Named Site Collections (HNSC), it is necessary to use a combination of host names and Server Name Identification (SNI) to route the incoming requests correctly.

You would think that with the ability to create a New-WebBinding in PowerShell, that you would have the ability to set all of your binding properties in one cmdlet, but you would be wrong.

[![](/images/new-binding.jpg)](/images/new-binding.jpg)

If you run the Get-Binding cmdlet for your site, will will see that there are properties for the certificate thumbprint (certificateHash) and certificate store location.

[![](/images/get-binding.jpg)](/images/get-binding.jpg)

Looking at the Set-Binding cmdlet, there is the ability to set PropertyNames and Values, but they will not allow you to update the thumbprint or certificate store location...

[![](/images/set-binding.jpg)](/images/set-binding.jpg)

So, how do we set these values through PowerShell:

[![](/images/add binding gui.jpg)](/images/add binding gui.jpg)

```
Import-Module WebAdministration -EA 0
# Prerequisites
# SAN Certificate must already be installed
# ALL Host Headers (DNS Names) must be in the Subject Alternative Name
# Variables
$site = "pcDemo 2013 Hosting Web App"      # IIS Site Name to bind certificates to
$certFriendlyName = "san.pcDemo.net"       # Certificate Friendly Name
$caHostHeader = "ca.pcDemo.net"            # Central Admin Host Header
# Certificate stuff
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint.ToString()
$dnsNames = $cert.DnsNameList | Select Unicode
foreach ($dnsName in $dnsNames) {
    $hostHeader = $dnsName.Unicode.ToString()
    if ($hostHeader -ne $caHostHeader) {
        Write-Host("Creating $hostHeader binding...")
        # Create the Binding
        New-WebBinding -Name $site -SslFlags 1 -HostHeader $hostHeader -Protocol https -Port 443 -IPAddress "*"
        Write-Host("Binding " + $cert.FriendlyName + " to $hostHeader...")
        # Add the Certificate
        New-Item -Path "IIS:\SslBindings\*!443!$hostHeader" -Thumbprint $thumbprint -SSLFlags 1
   }
}
```

Now, you are not truly done yet. Currently the site does not have a default SSL site as all of the bindings created so far all have Host Headers associated with them, which will create an error within IIS.

[![](/images/ssl sni error.jpg)](/images/ssl sni error.jpg)

For those of you implementing SharePoint, it is now time to bind your App Domain. This is the binding that will accept all incoming requests that do not have an SNI associated with them.

```
Import-Module WebAdministration -EA 0
# Prerequisites
# SAN Certificate must already be installed
# ALL Host Headers must be in the Subject Alternative Name as DNS Names
# Variables
$site = "pcDemo 2013 Hosting Web App"      # IIS Site Name to bind certificates to
$certFriendlyName = "san.pcDemo.net"       # Certificate Friendly Name
# Certificate stuff
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint.ToString()
# is there already a binding?
$bindings = Get-WebBinding -Name $site -Port 443 -Protocol https
$exists = $bindings | ? {$_.sslFlags -eq 0}
if ($exists) {
    Write-Host("Binding already exists...")
    Remove-WebBinding -Name $site -Port 443 -Protocol https -HostHeader $null
    Remove-Item -Path "IIS:\SslBindings\0.0.0.0!443"
    Write-Host("Binding removed...")
}
# Create new binding with correct certificate
# Create the Binding
New-WebBinding -Name $site -Protocol https -Port 443 -IPAddress "*"
Write-Host("Binding " + $cert.FriendlyName)
# Add the Certificate
New-Item -Path "IIS:\SslBindings\0.0.0.0!443" -Thumbprint $thumbprint

```

At this point the SharePoint Web Application (IIS site) should have all of its bindings in place. So what could possibly be left? Well, what about the Central Administration (CA) Web Application?

This one will be a combination of the last two scripts because CA will require SNI.

```
Import-Module WebAdministration -EA 0
# Prerequisites
# SAN Certificate must already be installed
# ALL Host Headers must be in the Subject Alternative Name as DNS Names
# Variables
$site = "SharePoint Central Administration v4"      # IIS Site Name to bind certificates to
$certFriendlyName = "san.pcDemo.net"                # Certificate Friendly Name
$caHostHeader = "ca.pcdemo.net"                     # Central Administration Host Header
# Certificate stuff
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $certFriendlyName)}
$thumbprint = $cert.Thumbprint.ToString()
Write-Host("Creating $caHostHeader binding...")
# Create the Binding
New-WebBinding -Name $site -SslFlags 1 -HostHeader $caHostHeader -Protocol https -Port 443 -IPAddress "*"
Write-Host("Binding " + $cert.FriendlyName + " to $caHostHeader...")
# Add the Certificate
New-Item -Path "IIS:\SslBindings\*!443!$caHostHeader" -Thumbprint $thumbprint -SSLFlags 1

```

Now to put the entire thing together:

```
Import-Module WebAdministration -EA 0
# Prerequisites
# SAN Certificate must already be installed
# ALL Host Headers (DNS Names) must be in the Subject Alternative Name
# Variables
$site = "pcDemo 2013 Hosting Web App"         # IIS Site Name for SharePoint Hosting
$sanCertFriendlyName = "san.pcDemo.net"       # SAN Certificate Friendly Name
$appCertFriendlyName = "pcdemo-apps.net"      # App Domain Wild Card Certificate Friendly Name
$caCertFriendlyName = "ca.pcDemo.net"         # Central Administration Certificate FriendlyName
$caHostHeader = "ca.pcdemo.net"               # Central Administration Host Header
#region Create SAN Bindings
# Certificate stuff
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $sanCertFriendlyName)}
$thumbprint = $cert.Thumbprint.ToString()
$dnsNames = $cert.DnsNameList | Select Unicode
foreach ($dnsName in $dnsNames) {
    $hostHeader = $dnsName.Unicode.ToString()
    if ($hostHeader -ne $caHostHeader) {
        Write-Host("Creating $hostHeader binding...")
        # Create the Binding
        New-WebBinding -Name $site -SslFlags 1 -HostHeader $hostHeader -Protocol https -Port 443 -IPAddress "*"
        Write-Host("Binding " + $cert.FriendlyName + " to $hostHeader...")
        # Add the Certificate
        New-Item -Path "IIS:\SslBindings\*!443!$hostHeader" -Thumbprint $thumbprint -SSLFlags 1
    }
}
#endregion
#region Create Default 443 Bindings
# Certificate stuff
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $appCertFriendlyName)}
$thumbprint = $cert.Thumbprint.ToString()
# is there already a binding?
$bindings = Get-WebBinding -Name $site -Port 443 -Protocol https
$exists = $bindings | ? {$_.sslFlags -eq 0}
if ($exists) {
    Write-Host("Binding already exists...")
    Remove-WebBinding -Name $site -Port 443 -Protocol https -HostHeader $null
    Remove-Item -Path "IIS:\SslBindings\0.0.0.0!443"
    Write-Host("Binding removed...")
}
# Create new binding with correct certificate
# Create the Binding
New-WebBinding -Name $site -Protocol https -Port 443 -IPAddress "*"
Write-Host("Binding " + $cert.FriendlyName)
# Add the Certificate
New-Item -Path "IIS:\SslBindings\0.0.0.0!443" -Thumbprint $thumbprint
#endregion
#region Create Central Administration Bindings
$site = "SharePoint Central Administration v4"      # IIS Site Name to bind certificates to
# Certificate stuff
$cert = Get-ChildItem -path cert:\LocalMachine\My | Where {($_.FriendlyName -eq $caCertFriendlyName)}
$thumbprint = $cert.Thumbprint.ToString()
Write-Host("Creating $caHostHeader binding...")
# Create the Binding
New-WebBinding -Name $site -SslFlags 1 -HostHeader $caHostHeader -Protocol https -Port 443 -IPAddress "*"
Write-Host("Binding " + $cert.FriendlyName + " to $caHostHeader...")
# Add the Certificate
New-Item -Path "IIS:\SslBindings\*!443!$caHostHeader" -Thumbprint $thumbprint -SSLFlags 1
#endregion

```

Updates

04/19/2015 Fixed paragraph 3 from Get-WebBinding to Set-WebBinding

04/21/2015 Added CA certificate checking and added Remove-Item to allow App Domain cert to be added with New-Item
