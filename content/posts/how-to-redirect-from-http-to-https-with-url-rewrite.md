---
title: "How to Redirect from HTTP to HTTPS with URL Rewrite"
date: 2013-10-20T01:37:00+0000
tags: ["URL Redirect", "HTTP", "IIS 7", "HTTPS", "URL Rewrite"]
aliases:
  - "/2013/10/how-to-redirect-from-http-to-https.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I ran into an issue with trying to trick IIS into redirecting using the method that I described in my previous blog post [HTTP to HTTPS Redirect in IIS7](/posts/http-to-https-redirect-in-iis7/). I tried to get my rewrite configured manually using the out of the box HTTP Redirect in IIS, but was not having much luck.**

[*](/images/OTB HTTP Redirect.jpg)
I did not have all day, so after looking around for a bit, I found a GUI that works with the HTTP Redirect module to make creating the redirect easier. This demonstration will be using [Microsoft URL Rewrite Module 2.0 for IIS 7 (x64)](http://www.microsoft.com/en-us/download/details.aspx?id=7435) with the update [Update for URL Rewrite Module 2.0 (KB2749660) (x64)](http://www.microsoft.com/en-us/download/details.aspx?id=34753). You will need to be an Administrator on the machine where you install the module. There are not any parameters to set during the installation, they are both Next-Next-Finish installations. However, the Update runs a repair installation module, which is still just an N-N-F install. You also have the option to use the [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx) to install the URL Rewrite module.

If you do not stop IIS before installing, a server reboot will be required.

After installation is complete, you should see the new module added to the sites in IIS

[![](/images/URL Rewrite Module.jpg)](/images/URL Rewrite Module.jpg)
Objective

Our goal is going to take a standard HTTP request for http://sp2010.contoso.com and redirect it to https://sp2010.contoso.com.

Let's Get Started

Please remember that this is a GUI for writing information into your web.config file. It is always best to make a copy of your web.config file before making any changes (GUI based or manually).

To get started, double click on the URL Rewrite module, and select Add Rule(s)... 

[![](/images/Add Rules.jpg)](/images/Add Rules.jpg)

which will open a window to select the type of rule template to use.

[![](/images/Add Rules Template.png)](/images/Add Rules Template.png)

Start by naming your rule...  Be descriptive as you never know what else you might add at a later date... Then update the Match URL section to match the image below.

[![](/images/Match URL.png)](/images/Match URL.png)

If you press the Test pattern...* button, and enter a URL such as *http://sp2010.contoso.com/sites/sales*, take notice of the Capture groups, as you will see the Back Reference used in an upcoming setting. The important take away is that the values of the Back References are for the exact URL that you entered, meaning that the entire URL is ready for the next step in the Redirect.

[*](/images/Match URL Test.png)

After closing out the Test Pattern window, in the Conditions section, click the Add... *button to create a condition for the redirect rule and set the parameters as seen in the image below.

[*](/images/Add Condition.png)

The Test Pattern for this condition will always fail as it does not test the URI scheme (HTTP or HTTPS).

There are not any changes or additions required for the Server Variables section.

In the Action section, set the parameters based off the image below.

[![](/images/Actions.png)](/images/Actions.png)

When you are done entering the parameters, click the Apply* link and then click the *Back to Rules* link.

[*](/images/Apply and Back.png)

If you have more questions about URL Rewrite*** and how it works, the [Online Help link](http://www.iis.net/learn/extensions/url-rewrite-module/url-rewrite-module-configuration-reference?amp;clcid=0x409) is very useful.

After pressing *Apply*, and *Back*, your URL Rewrite rule should look something like this:

[*](/images/URL Rewrite Rule Complete.png)

Let's Clean Things Up

You now have the ability to redirect, but have you set your bindings in IIS? And if you are using SharePoint, have you set your Alternate Access Mappings? Don't forget that you will also need an SSL Certificate (preferably a SAN certificate) so that you can create your port 443 binding. Remember that out of the box, IIS 7 will only allow one (1) port 443 binding per server. Please read my other post on how to host more than one URL with port 443 bindings on the same IP address (coming soon).

Troubleshooting

There are a couple of things that you need to keep in mind when using a redirect. 

- You will still need to have the port 80 binding enabled.
- Under the site's SSL Settings, the Require SSL check box should NOT be selected.

Behind the Scenes

The URL Rewrite module is a nice tool that keeps you physically out of your web.config file. As a second reminder, URL Rewrite is a GUI for writing information into your web.config file. It is always best to make a copy of your web.config file before making any changes (GUI based or manually). 

After you hit the Apply* link, this is what has been added to the IIS site's web.config file:

[![](/images/web.config.png)](/images/web.config.png)

If you wish to add the redirect manually, copy/paste below to your web.config file (after backing it up first).

```

  
    
      
        
        
          
        
        
      
    
  

```

Update 10/20/2013: Added the web.config information for copy/paste

Update 01/05/2015: Added the link to reference information to download via Web Platform Installer
