---
title: "SharePoint and FIPS Exceptions"
date: 2015-06-28T22:29:00+0000
categories: ["PowerShell", "SharePoint"]
tags: ["Algorithms", "Cryptographic", "FIPS"]
aliases:
  - "/2015/06/sharepoint-and-fips-exceptions.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

A couple of weeks ago, I started a "Greenfield" implementation of SharePoint 2013 for a client. This organization has SharePoint 2003, 2007, 2010 already existing in their environment, so I ignorantly figured that the installation should go pretty smoothly.**
All of the SharePoint and SQL bits installed correctly, however when trying to provision Central Administration, I ran into an issue where I was not able to create the config database:

[*](/images/Config Error.png)

### 
What is FIPS?

FIPS stands for the Federal Information Processing Standards**, and is used for the standardization of information, such as FIPS 10-4 for Country Codes or FIPS 5-2 for State Codes. However my problem is with FIPS 140-2, the Security Requirements for Cryptography which states:

> This Federal Information Processing Standard (140-2) specifies the security requirements that will be satisfied by a cryptographic module, providing four increasing, qualitative levels intended to cover a wide range of potential applications and environments. The areas covered, related to the secure design and implementation of a cryptographic module, include specification; ports and interfaces; roles, services, and authentication; finite state model; physical security; operational environment; cryptographic key management; electromagnetic interference/electromagnetic compatibility (EMI/EMC); self-tests; design assurance; and mitigation of other attacks. [Supersedes FIPS 140-1 (January 11, 1994): http://www.nist.gov/manuscript-publication-search.cfm?pub_id=917970]*

*[http://www.nist.gov/manuscript-publication-search.cfm?pub_id=902003](http://www.nist.gov/manuscript-publication-search.cfm?pub_id=902003)*

In essence, FIPS 140-2 is a standard that can be tested against and certified so that the server is hardened up to a government standard. Currently, the US is not the only government that uses the FIPS standard for server hardening. The FIPS Local/Group Security Policy Flag can be found here:

[![](/images/enable FIPS.png)](/images/enable FIPS.png)

### 
FIPS and SharePoint

There are a couple of problems with using SharePoint on a FIPS enabled server. SharePoint Server uses MD5 for computing hash values (not for security purposes) which is an unapproved algorithm. According to Microsoft ([https://technet.microsoft.com/en-us/library/cc750357.aspx](https://technet.microsoft.com/en-us/library/cc750357.aspx)) Schannel Security Package is forced to negotiate sessions using TLS1.0. And the following supported Cipher Suites are disabled:

- TLS_RSA_WITH_RC4_128_SHA
- TLS_RSA_WITH_RC4_128_MD5
- SSL_CK_RC4_128_WITH_MD5
- SSL_CK_DES_192_EDE3_CBC_WITH_MD5
- TLS_RSA_WITH_NULL_MD5
- TLS_RSA_WITH_NULL_SHA

If you want to read up more, here are some good posts:

- [Don’t Even Bother Trying to Enforce FIPSAlgorithmPolicy on SharePoint 2010 Servers](http://blogs.msdn.com/b/chaun/archive/2014/04/09/don-t-even-bother-trying-to-enforce-fipsalgorithmpolicy-on-sharepoint-2010-servers.aspx)
- [FIPS 140 Validation](https://technet.microsoft.com/en-us/library/cc750357.aspx)
- [FIPS PUB 140-2- Security Requirements for Cryptographic Modules](http://csrc.nist.gov/groups/STM/cmvp/standards.html)
- [Why We’re Not Recommending “FIPS Mode” Anymore](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

### 
What's Next?

Disabling FIPS is easy, however a larger discussion needs to be had. Is FIPS set at the GPO level or is it part of the image that was provisioned and FIPS was enabled by default? Will the security team come after you if you disable it without their knowledge? Why do they have FIPS enabled, and what are they trying to accomplish with FIPS? All of these questions will need to be answered before changing your server settings.

### 
Fixing FIPS with PowerShell

This is how I reset the FIPS Algorithm Policy so that I could get Central Administration provisioned. Remember that FIPS will need to be disabled on all of your SharePoint Servers.

```
$sets = @("CurrentControlSet","ControlSet001","ControlSet002")
foreach ($set in $sets) {
    $path = "HKLM:\SYSTEM\$set\Control\LSA\FipsAlgorithmPolicy"
    if ((Get-ItemProperty -Path $path).Enabled -ne 0) {
        Set-ItemProperty -Path $path -Name "Enabled" -Value "0"
        Write-Host("Set $path Enabled to 0")
    }
}

```
