---
title: "How To Create an HTTPS Friendly URL for Your Active Directory Certificate Services (ADCS)"
date: 2013-10-21T19:41:00+0000
tags: ["URL Redirect", "HTTP Redirect", "HTTPS", "Certificate Services", "HTTP", "ADCS", "Active Directory"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

After adding the Active Directory Certificate Services (ADCS) role to your server, you will be able to open up your browser and request a certificate. The problem is that the URL is based off the server name and you have to remember the site name. For example to get to the Certificate Authority (CA) on my backup domain controller, I would have to go to *http://dc02/certsrv/default.asp*. There are a couple of problems with this URL; one is that it is over HTTP and the other is that I will not remember the URL. Granted, I can always save the URL to my Favorites, but that is not how I roll, and I prefer to keep things simple so that I can remember URLs.

Objective 

The objective of this demonstration is to show how to create a safer, and easier way to get to the Welcome page of the Active Directory Certificate Services web site.

Let's Get Started

As with all things new, it is best to start off this exercise and validate that you can get to your ADCS site on port 80. Once you have your port 80 site up and running, it will be time to create a friendly named URL, such as ca.pcDemo.net. You will want to make sure that the URL meets your naming scheme for internal (inside the firewall) URLs as you do not want to expose your ADCS server to the outside world. After you have come up with a good name, and your boss has OK'd his URL, have a New Host(A) name created. Once the new host name has been added to DNS, validate...

To get the CA on port 443 without error, a certificate will need to be created for the Name entered into DNS. On the ADSI server, open up IIS Manager (Windows Key + R --> inetmgr), select server name and double click on Server Certificates under the IIS Module.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj3saM5c8wHzbpmA_KT0N7EzkZIAugwi-JKZieqnPncplgszcwXqJsVgm728aq1cxgivmRigRQYq_RDP8SS5sfHBhcWxgsaf7oL8Z0gumRcstm5EEeGb30eOMhWdAL9mDwdfOD10XrgFTRI/s1600/Open+Server+Certificates.png)

From the Server Certificates page, select the Create Domain Certificate...*

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjWU0nnKKUjUeUlh3T8Te1G69waZ6UDICpNa8y_vy5pwU6v-666HYJRJq2rDto1YhzOHapVNKbfdVNH0ugsgZCEwftmw1bpkokEpX93xBxZMFN_ATV5QfJZk-oY54gUVH6GvHi7orf5Oix_/s1600/Create+Domain+Cert.png)

Once the Distinguished Name Properties window opens, fill it out correctly.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgWXOY4UM_bUdw8tEioe60f_3tSra6f5knPTZpZmycGqh5cMjYI9hQam4B0qUHafpLN620jiP5LCVktgqbxKGakwXwyh7BPvETSqUo_rILm9Oi3rXZvvGFdUaNMd6Jo3z3rzcWecyd3Q1iL/s640/Create+Cert+1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgWXOY4UM_bUdw8tEioe60f_3tSra6f5knPTZpZmycGqh5cMjYI9hQam4B0qUHafpLN620jiP5LCVktgqbxKGakwXwyh7BPvETSqUo_rILm9Oi3rXZvvGFdUaNMd6Jo3z3rzcWecyd3Q1iL/s1600/Create+Cert+1.png)

Click the Next* button to continue, which will bring up Online Certification Authority winds. Click the *Select *button and find the CA that you wish to use to supply the certificate. Then put in your friendly name for the certificate.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEirbAI-XCGuiZuN3tMJxBqhxS8LMbgPbnqoagyEf-VfHDaNsArvWTCXbj2lPmJwC8Pork930ImHWXtAZjZiz246BAVfeVz0r7jbCxqxuP3QMpg6QxBrQWygbJR-PLdLl7Ov5s5Sq5f6KytQ/s1600/Create+Cert+2.png)

Click the Finish *button to complete the certificate request. If you have the appropriate permissions, the certificate should have been created and added to the list of available server certificates.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgdfwgPLEl00ckK6jd-f1Ufws18l2zcdyjKulEpfvKWvcGs7riNK8QmFg4veQhIumEW2p79Gkf0u47OKcJSUxt3LupSA1NV_lc6LhJZSDw_XapEQo5I1Pm09pjaHnYLeYCSTr7BNzNZZFCz/s1600/Certificate+Added.png)

The next step is to create the binding to the Default Web Site.

Start by selecting the Default Web Site, then select Binding from the Edit Site Actions section.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-az3xPk1K53iEFu0Tmel7TXJNWdP_T-hSpJGG1xIEMZoOMItxYREa0yjAy8-NK_vFS-3OUQ1hJLkRYeHQ-CJuVboDoM6SFlAcPeazDQ2JKGR7M_9VU9AJ0ecSuELHCiSeXcZ7WMeVc5SK/s640/Open+Bindings.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-az3xPk1K53iEFu0Tmel7TXJNWdP_T-hSpJGG1xIEMZoOMItxYREa0yjAy8-NK_vFS-3OUQ1hJLkRYeHQ-CJuVboDoM6SFlAcPeazDQ2JKGR7M_9VU9AJ0ecSuELHCiSeXcZ7WMeVc5SK/s1600/Open+Bindings.png)

From the Site Bindings window, click the Add...* button. From the Add Site Binding window, change the type of connection to HTTPS, and select the appropriate certificate from the SSL certificate drop-down.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhMxiCpeP3CG0hZSyeEdPtLhOiMkTz5PYdvpus8VOMT8Vjncf55XTID1lkUEX3TN5OkWEAbFAyWpVWrf00s6Q0P0vv7NgDivO3hsBuT3bN7-I0B2cl1t22XH9_hRaQmeunUGtNueFlls3fq/s1600/Add+Site+Binding.png)

Depending on your version of IIS, either the Host name will be grayed out or not. This is running on IIS 8.5 on Server 2012 R2.

At this point your should be able to open up your browser to https://<friendlyname>/certsrv 

IMPORTANT: You will not be able to log-in to the site from your ADCS server. You will want to test from another machine on the domain.

Create the Redirect

Now there is the final step to make life easier for you and your clients, and that is to have a friendly URL name that will be redirected to the CA page on port 443.

Again, select the Default Web Site, and double click on the HTTP Redirect module.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmIEC68cRer_jA0G0Z_bM7oxbHk-MgtLj8avVv1I_dO7wQ5ZLiYLosKMrC5OB-bY2qFcFFdrZV436e56s_-WH9wCMAACFyb1wRnj2UNMVDUSjW3ze6HmLbPSr8OBjWVuDqnXPthih3PMYO/s640/HTTP+Redirect.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmIEC68cRer_jA0G0Z_bM7oxbHk-MgtLj8avVv1I_dO7wQ5ZLiYLosKMrC5OB-bY2qFcFFdrZV436e56s_-WH9wCMAACFyb1wRnj2UNMVDUSjW3ze6HmLbPSr8OBjWVuDqnXPthih3PMYO/s1600/HTTP+Redirect.png)

This will bring up the HTTP Redirect window. Enable the redirect to the appropriate location and select the appropriate behavior, and status code.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGOikl8iqTLe2qXJFjWywVcCoZQmEeubjq1hdilcC1ITB8voB5eatJAOarUeXf5dceK14kREpiazWKVGgeepfhN3koMO2cYVA6O3BrRsH-uKiVjjuhxG0Cv9M-K9CafkD_Jkneo-DScEzr/s640/HTTPS+Redirect.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhGOikl8iqTLe2qXJFjWywVcCoZQmEeubjq1hdilcC1ITB8voB5eatJAOarUeXf5dceK14kREpiazWKVGgeepfhN3koMO2cYVA6O3BrRsH-uKiVjjuhxG0Cv9M-K9CafkD_Jkneo-DScEzr/s1600/HTTPS+Redirect.png)

After you click the Apply* link, IIS will create a web.config file for you. IIS has added the following into your web.config file:

```

    
        
    

```

And now, from a browser not located on the certificate server, you should be able to go to http://<friendlyname> and automatically get redirected to https://<friendlyname>/certsrv
