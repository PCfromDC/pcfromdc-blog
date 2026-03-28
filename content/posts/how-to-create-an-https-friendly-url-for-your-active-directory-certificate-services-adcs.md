---
title: "How To Create an HTTPS Friendly URL for Your Active Directory Certificate Services (ADCS)"
date: 2013-10-21T19:41:00+0000
tags: ["URL Redirect", "HTTP Redirect", "HTTPS", "Certificate Services", "HTTP", "ADCS", "Active Directory"]
aliases:
  - "/2013/10/how-to-create-an-https-friendly-url.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

After adding the Active Directory Certificate Services (ADCS) role to your server, you will be able to open up your browser and request a certificate. The problem is that the URL is based off the server name and you have to remember the site name. For example to get to the Certificate Authority (CA) on my backup domain controller, I would have to go to *http://dc02/certsrv/default.asp*. There are a couple of problems with this URL; one is that it is over HTTP and the other is that I will not remember the URL. Granted, I can always save the URL to my Favorites, but that is not how I roll, and I prefer to keep things simple so that I can remember URLs.

Objective 

The objective of this demonstration is to show how to create a safer, and easier way to get to the Welcome page of the Active Directory Certificate Services web site.

Let's Get Started

As with all things new, it is best to start off this exercise and validate that you can get to your ADCS site on port 80. Once you have your port 80 site up and running, it will be time to create a friendly named URL, such as ca.pcDemo.net. You will want to make sure that the URL meets your naming scheme for internal (inside the firewall) URLs as you do not want to expose your ADCS server to the outside world. After you have come up with a good name, and your boss has OK'd his URL, have a New Host(A) name created. Once the new host name has been added to DNS, validate...

To get the CA on port 443 without error, a certificate will need to be created for the Name entered into DNS. On the ADSI server, open up IIS Manager (Windows Key + R --> inetmgr), select server name and double click on Server Certificates under the IIS Module.

[*](/images/Open Server Certificates.png)

From the Server Certificates page, select the Create Domain Certificate...*

[*](/images/Create Domain Cert.png)

Once the Distinguished Name Properties window opens, fill it out correctly.

[![](/images/Create Cert 1.png)](/images/Create Cert 1.png)

Click the Next* button to continue, which will bring up Online Certification Authority winds. Click the *Select *button and find the CA that you wish to use to supply the certificate. Then put in your friendly name for the certificate.

[*](/images/Create Cert 2.png)

Click the Finish *button to complete the certificate request. If you have the appropriate permissions, the certificate should have been created and added to the list of available server certificates.

[*](/images/Certificate Added.png)

The next step is to create the binding to the Default Web Site.

Start by selecting the Default Web Site, then select Binding from the Edit Site Actions section.

[![](/images/Open Bindings.png)](/images/Open Bindings.png)

From the Site Bindings window, click the Add...* button. From the Add Site Binding window, change the type of connection to HTTPS, and select the appropriate certificate from the SSL certificate drop-down.

[*](/images/Add Site Binding.png)

Depending on your version of IIS, either the Host name will be grayed out or not. This is running on IIS 8.5 on Server 2012 R2.

At this point your should be able to open up your browser to https://<friendlyname>/certsrv 

IMPORTANT: You will not be able to log-in to the site from your ADCS server. You will want to test from another machine on the domain.

Create the Redirect

Now there is the final step to make life easier for you and your clients, and that is to have a friendly URL name that will be redirected to the CA page on port 443.

Again, select the Default Web Site, and double click on the HTTP Redirect module.

[![](/images/HTTP Redirect.png)](/images/HTTP Redirect.png)

This will bring up the HTTP Redirect window. Enable the redirect to the appropriate location and select the appropriate behavior, and status code.

[![](/images/HTTPS Redirect.png)](/images/HTTPS Redirect.png)

After you click the Apply* link, IIS will create a web.config file for you. IIS has added the following into your web.config file:

```

    
        
    

```

And now, from a browser not located on the certificate server, you should be able to go to http://<friendlyname> and automatically get redirected to https://<friendlyname>/certsrv
