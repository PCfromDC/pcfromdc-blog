---
title: "Http to Https Redirect in IIS7"
date: 2011-02-10T20:31:00+0000
tags: ["Custom Page", "Redirecting", "SSL", "IIS 7"]
aliases:
  - "/2011/02/http-to-https-redirect-in-iis7.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

To keep with the subject of error pages, I thought it would be good to touch on how to redirect end users and force them to use port 443 instead of port 80.  There is a very simple way to accomplish this in IIS7, using the same Error Pages Feature that we used in the last posting on creating a [SharePoint Custom Login Error (401) Page](/posts/sharepoint-custom-login-error-401-page/). 

1) Go to IIS and select your web site.

2) If you have not already done so, edit the site bindings to add the port 443

3) Open the Error Pages Feature under the IIS area.  In the right column, click Add.[![](/images/Add Error Page.jpg)](/images/Add Error Page.jpg)

4) Add a Status Code of 403.4 and select Respond with a 302 redirect.  Put in YOUR https address![![](/images/Create custom error page.jpg)](/images/Create custom error page.jpg)

[![](/images/Should look like.jpg)](/images/Should look like.jpg)

5) Open the SSL Settings in the IIS area for your site.

[![](/images/SSL Settings Selected.jpg)](/images/SSL Settings Selected.jpg)

6) Click the Require SSL check box, and click Apply in the upper right Actions column.[![](/images/Force SSL.jpg)](/images/Force SSL.jpg)
