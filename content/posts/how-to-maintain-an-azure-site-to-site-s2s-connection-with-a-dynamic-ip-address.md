---
title: "How to Maintain an Azure Site-to-Site (S2S) Connection with a Dynamic IP Address"
date: 2017-12-15T21:25:00+0000
categories: ["PowerShell", "Azure"]
tags: ["RRAS", "VPN", "S2S", "Site to Site"]
aliases:
  - "/2017/12/how-to-maintain-an-azure-site-to-site.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

This post should not be needed for your production environment. This is for those of us who test and build development environments at home and have created hybrid environments into Azure. IF you have a dynamic IP address for your business, please spend the extra bit of money for a static IP...

That being said, I have a dynamic IP address for my house, and when my IP address changes, it used to break my S2S connection with Azure. This post is about how I fixed the problem.

The fist step is to create the service account that is going to be logging in to Azure to check and update the IP Address.  I will be creating an unlicensed user on the .onmicrosoft domain to for this purpose.

As the Microsoft Online Data Service (MSOL) module did not come pre-installed, I ran the following to get started:

Next we are going to login and create the unlicensed service account. You will want to update the UPN and other variables accordingly:

Now that we have our service account created (an account that does not have access into our domain, O365, or Azure), it will need to be added to Access Control (IAM) for the Local Network Gateway in Azure.

With permissions set for Local Network Gateway, it is time to look at the current IP address of the gateway endpoint and compare it to the current local IP address endpoint. If the two IP addresses do not match, it is time to update your Local Network Gateway (in Azure).

Next we create some logging and logging clean-up:

And to finish off, we will connect all of our RRAS VpnS2SInterface connections. 

Now let's put the whole thing together. First we create the service account and add their permissions:

Next we create the Update S2S file, and save the file to: 'C:\Scripts\Update S2S and RRAS.ps1'

Now that we are checking and updating our Local Network Gateway Connection IP address, we need to create a timer job that will check and update on a regular basis. Below is a script that will check every hour on the hour. Make sure that the Update S2S file path is set correctly.
