---
title: "How to Redirect from HTTP to HTTPS with URL Rewrite"
date: 2013-10-20T01:37:00+0000
tags: ["URL Redirect", "HTTP", "IIS 7", "HTTPS", "URL Rewrite"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I ran into an issue with trying to trick IIS into redirecting using the method that I described in my previous blog post [HTTP to HTTPS Redirect in IIS7](http://pcfromdc.blogspot.com/2011/02/http-to-https-redirect-in-iis7.html). I tried to get my rewrite configured manually using the out of the box HTTP Redirect in IIS, but was not having much luck.**

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgN6KiufEAHywY0X_3kHneajo7ttMGe72jZTyV__4pm-IYLENQwa_6LmqiUUls5COFD2tm0eX7W_2m4wdim6qQbEvMXUBymaxroXlj1PATmIc0j-ITisaJc7Ijx3LGybBxmixlcUO-K1MsH/s1600/OTB+HTTP+Redirect.jpg)
I did not have all day, so after looking around for a bit, I found a GUI that works with the HTTP Redirect module to make creating the redirect easier. This demonstration will be using [Microsoft URL Rewrite Module 2.0 for IIS 7 (x64)](http://www.microsoft.com/en-us/download/details.aspx?id=7435) with the update [Update for URL Rewrite Module 2.0 (KB2749660) (x64)](http://www.microsoft.com/en-us/download/details.aspx?id=34753). You will need to be an Administrator on the machine where you install the module. There are not any parameters to set during the installation, they are both Next-Next-Finish installations. However, the Update runs a repair installation module, which is still just an N-N-F install. You also have the option to use the [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx) to install the URL Rewrite module.

If you do not stop IIS before installing, a server reboot will be required.

After installation is complete, you should see the new module added to the sites in IIS

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiP4KJeytvBznAk5y_rN03yklCo88RTZ3SUq0jIe8yRL9II58t219JeVNtm3df6RkSXSluHvzX0DskhGw5HVpoW3r8jnVQpnrfLIknrycy_oWcclDZ1ZWwC9p5Lq70mwSWmJcc9PLJcZLht/s640/URL+Rewrite+Module.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiP4KJeytvBznAk5y_rN03yklCo88RTZ3SUq0jIe8yRL9II58t219JeVNtm3df6RkSXSluHvzX0DskhGw5HVpoW3r8jnVQpnrfLIknrycy_oWcclDZ1ZWwC9p5Lq70mwSWmJcc9PLJcZLht/s1600/URL+Rewrite+Module.jpg)
Objective

Our goal is going to take a standard HTTP request for http://sp2010.contoso.com and redirect it to https://sp2010.contoso.com.

Let's Get Started

Please remember that this is a GUI for writing information into your web.config file. It is always best to make a copy of your web.config file before making any changes (GUI based or manually).

To get started, double click on the URL Rewrite module, and select Add Rule(s)... 

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgSQghTk_d0CLcpOvBB2NAm65e1FSzRdoqfbKHOjjm3RpALvgtQzyZTa_v81QswwEyPShdRRJEx2e6RYcA7QOqXCGbKtiL1LWnxuxemJrG0rW-I3J6b5eO0A0tYAIEOZDIxNfmrqG7Z_GFE/s640/Add+Rules.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgSQghTk_d0CLcpOvBB2NAm65e1FSzRdoqfbKHOjjm3RpALvgtQzyZTa_v81QswwEyPShdRRJEx2e6RYcA7QOqXCGbKtiL1LWnxuxemJrG0rW-I3J6b5eO0A0tYAIEOZDIxNfmrqG7Z_GFE/s1600/Add+Rules.jpg)

which will open a window to select the type of rule template to use.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhYHEwJFNSxn0HMhKN8n1hKP1c6ycP7bQD5xkznM9-g7D1SCc2JbXp2rXWx8Oilje118FxDR-ljP-iNGphUvIFDQKRQmJIV9TzOSvZhyODh-GzJHvhO7sesSx7_tj7FS7k6wwyiIzW-jOp3/s640/Add+Rules+Template.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhYHEwJFNSxn0HMhKN8n1hKP1c6ycP7bQD5xkznM9-g7D1SCc2JbXp2rXWx8Oilje118FxDR-ljP-iNGphUvIFDQKRQmJIV9TzOSvZhyODh-GzJHvhO7sesSx7_tj7FS7k6wwyiIzW-jOp3/s1600/Add+Rules+Template.png)

Start by naming your rule...  Be descriptive as you never know what else you might add at a later date... Then update the Match URL section to match the image below.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRwNW7_WUN-OzDHJr0f_HIYmo7oFAm-B-0JMaPOqU3zXyKBXB6uqx_vTEFR10cRyHcZwNMlRjkjQtpT81L0Ut4bnELBmjoClxwGEaz_BdekrM5tvDpKH-O9PJ06ord-OymBJ6G9J9i3ICZ/s640/Match+URL.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjRwNW7_WUN-OzDHJr0f_HIYmo7oFAm-B-0JMaPOqU3zXyKBXB6uqx_vTEFR10cRyHcZwNMlRjkjQtpT81L0Ut4bnELBmjoClxwGEaz_BdekrM5tvDpKH-O9PJ06ord-OymBJ6G9J9i3ICZ/s1600/Match+URL.png)

If you press the Test pattern...* button, and enter a URL such as *http://sp2010.contoso.com/sites/sales*, take notice of the Capture groups, as you will see the Back Reference used in an upcoming setting. The important take away is that the values of the Back References are for the exact URL that you entered, meaning that the entire URL is ready for the next step in the Redirect.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhyKfvNcYd7QiNIT9owPsm3JqL13PfNYLNqt4i4s7uhP3jWkgVLoxv_we0v7C3nLM2OHLeRqcKAj0LOR9M56lsm1-as4-eXptWwpdb9xHf31oNcPJgtwir0JVn1F_t5RG9s3j3tbgRfOm1o/s1600/Match+URL+Test.png)

After closing out the Test Pattern window, in the Conditions section, click the Add... *button to create a condition for the redirect rule and set the parameters as seen in the image below.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZlmDghJdzx5443dxNjde1fVrIYUMI9psnfKVYdxWEYE26qdOzhwDWq5sPJyxedZk9zupG1m5kOlJovi05G6WYHTTnXSVMX1fdxCcnYiTiR7FC0cXSYEaOk6V4lMN6NpKKbErxTyxEk66F/s1600/Add+Condition.png)

The Test Pattern for this condition will always fail as it does not test the URI scheme (HTTP or HTTPS).

There are not any changes or additions required for the Server Variables section.

In the Action section, set the parameters based off the image below.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKJwyg_rL5CH6arDKdB_SK8g3M9zbaoaRPyA_gxcO6VXZ_G4mELcy3YWKeKLDSFyiN5YBaYgXw19UrkxUNam5CnIGqoPtSowrurJ575nwpPSRI5D-fsWi3961G9srYwrJt8_KjB_jKW_n1/s640/Actions.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKJwyg_rL5CH6arDKdB_SK8g3M9zbaoaRPyA_gxcO6VXZ_G4mELcy3YWKeKLDSFyiN5YBaYgXw19UrkxUNam5CnIGqoPtSowrurJ575nwpPSRI5D-fsWi3961G9srYwrJt8_KjB_jKW_n1/s1600/Actions.png)

When you are done entering the parameters, click the Apply* link and then click the *Back to Rules* link.

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjD1Ce-8L-X6ZEkD43GltDQltuw97znxEmaK8mpQgwat5fUWVqB4qTWmDjdAF8cxxgTsnSfXNp1r_-8RUOsI2jU3N-4A3GSK9GQ9nGnGzDxFD9k1jZ-mzbbj5OVv3gVv7L_BGOCS3ofiyJI/s1600/Apply+and+Back.png)

If you have more questions about URL Rewrite*** and how it works, the [Online Help link](http://www.iis.net/learn/extensions/url-rewrite-module/url-rewrite-module-configuration-reference?amp;clcid=0x409) is very useful.

After pressing *Apply*, and *Back*, your URL Rewrite rule should look something like this:

[*](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhyyzq5i7wtfk179QR0mP8JwoMHDKy9aADAaZoFdLXnnOYqYWM-WHFw_cT6hqxDaRBGoELmKSiWuF8cDN4Al-hrsla9vxy3aK17q7XGId1I8Jw56t66wDiSQeHcBcr8yQMaV5gYUTDAitQ-/s1600/URL+Rewrite+Rule+Complete.png)

Let's Clean Things Up

You now have the ability to redirect, but have you set your bindings in IIS? And if you are using SharePoint, have you set your Alternate Access Mappings? Don't forget that you will also need an SSL Certificate (preferably a SAN certificate) so that you can create your port 443 binding. Remember that out of the box, IIS 7 will only allow one (1) port 443 binding per server. Please read my other post on how to host more than one URL with port 443 bindings on the same IP address (coming soon).

Troubleshooting

There are a couple of things that you need to keep in mind when using a redirect. 

- You will still need to have the port 80 binding enabled.
- Under the site's SSL Settings, the Require SSL check box should NOT be selected.

Behind the Scenes

The URL Rewrite module is a nice tool that keeps you physically out of your web.config file. As a second reminder, URL Rewrite is a GUI for writing information into your web.config file. It is always best to make a copy of your web.config file before making any changes (GUI based or manually). 

After you hit the Apply* link, this is what has been added to the IIS site's web.config file:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgNqToPtf7nn1qlsAJKIKW29u7ghhn_SDVcsN9curdsuDEaAJOic1Z06xVzEPJryGxPMAb_TNXV_7o3dsGI0mI4pobFuePxKQOE-WIG3s5r7URVGTBIyqXJU3QSDRUDRMpd2RJay78cLbX_/s640/web.config.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgNqToPtf7nn1qlsAJKIKW29u7ghhn_SDVcsN9curdsuDEaAJOic1Z06xVzEPJryGxPMAb_TNXV_7o3dsGI0mI4pobFuePxKQOE-WIG3s5r7URVGTBIyqXJU3QSDRUDRMpd2RJay78cLbX_/s1600/web.config.png)

If you wish to add the redirect manually, copy/paste below to your web.config file (after backing it up first).

```

  
    
      
        
        
          
        
        
      
    
  

```

Update 10/20/2013: Added the web.config information for copy/paste

Update 01/05/2015: Added the link to reference information to download via Web Platform Installer
