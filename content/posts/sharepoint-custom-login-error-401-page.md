---
title: "SharePoint Custom Login Error (401) Page"
date: 2011-02-10T14:27:00+0000
categories: ["SharePoint"]
tags: ["Custom Page", "Login Error", "Web Config", "IIS 7"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

When dealing with lots of people logging into your SharePoint site, you will spend a lot of time answering phone calls from people with login errors. A nice and easy way to preemptively help deal with login failures is to use a custom error page.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhBGHHFpNGZsHyuHYPoAlH31A-S3-mQ8qXdl8dO0myYKRND7NSCHz7RXG1SX2NZStPkUxzb08eGgpjpkTdzwl07gCTcGZhWs7KC-Cq_kfK8Y4wC8R4NRqoIIMymdRpz1O4AWJHhVYRroGPh/s640/Message.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhBGHHFpNGZsHyuHYPoAlH31A-S3-mQ8qXdl8dO0myYKRND7NSCHz7RXG1SX2NZStPkUxzb08eGgpjpkTdzwl07gCTcGZhWs7KC-Cq_kfK8Y4wC8R4NRqoIIMymdRpz1O4AWJHhVYRroGPh/s1600/Message.jpg)

1) Go to your IIS and look up where the custom errors are located for the site.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgzk2PFbtpX0bNQN_0DgojUrQvIYzlIl9daMpK-jRrb1PvcwgzxA2rkkLOPwBpZkpE-LAgG8-JcKuyC_vt9dm53fMF9mj5jcu0U6lSuJuRqqFgiQJsSWVcDVYugn8f6xNynKMjsxQ0Yx9d7/s640/IIS+Site.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgzk2PFbtpX0bNQN_0DgojUrQvIYzlIl9daMpK-jRrb1PvcwgzxA2rkkLOPwBpZkpE-LAgG8-JcKuyC_vt9dm53fMF9mj5jcu0U6lSuJuRqqFgiQJsSWVcDVYugn8f6xNynKMjsxQ0Yx9d7/s1600/IIS+Site.jpg)

2) Look at the path location for the location of the error files.[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhQkpABse4Htd1lb72ewvPbarxYDDOxpjR_xhmXGiv1WKgC2MQ0qxOZGfqPYoenk2D-KHC4_-tmehH_8BbDFixM4NtHiLW7Ltzz4LMiVoAoGMQfuXfYzE8HFShbDlN_XEW7ZhOJFMKTzJkW/s640/error+page+locations.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhQkpABse4Htd1lb72ewvPbarxYDDOxpjR_xhmXGiv1WKgC2MQ0qxOZGfqPYoenk2D-KHC4_-tmehH_8BbDFixM4NtHiLW7Ltzz4LMiVoAoGMQfuXfYzE8HFShbDlN_XEW7ZhOJFMKTzJkW/s1600/error+page+locations.jpg)

3) Go to the file location,, you will want to edit the 401.htm file.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjU1OH87bd3w5_VxB10OO9T7twKESRCWs7LAVe3JTC90o3Ap607VNUfPWE4CfPoXmCZBGLXZLZqN4AcAXdsnfc-VySK7eXzJE3uQUUUKMlZoqrV0cJn0vIQ4_Tb5yhJen2cV6P3LQ4D2tZl/s640/errror+pages.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjU1OH87bd3w5_VxB10OO9T7twKESRCWs7LAVe3JTC90o3Ap607VNUfPWE4CfPoXmCZBGLXZLZqN4AcAXdsnfc-VySK7eXzJE3uQUUUKMlZoqrV0cJn0vIQ4_Tb5yhJen2cV6P3LQ4D2tZl/s1600/errror+pages.jpg)

4) The last step is to modify the web.config for your SharePoint site.  You will need to add (modify) the system.webserver.[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiSVKLsgLQ6JvSBZQjrgxPnhSgnjph67UW3jX2JBlZWWqTL1QV09QHt_d-TbuAp_rHZXn6vHHCpOz8s-iw5qXb39I9W_14zDeM060vGTUITydinA1BnONeoKn27WRt9W3TzfYh5NCl5fQOk/s640/Web+Config.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiSVKLsgLQ6JvSBZQjrgxPnhSgnjph67UW3jX2JBlZWWqTL1QV09QHt_d-TbuAp_rHZXn6vHHCpOz8s-iw5qXb39I9W_14zDeM060vGTUITydinA1BnONeoKn27WRt9W3TzfYh5NCl5fQOk/s1600/Web+Config.jpg)

[Download code here](http://cid-5fd92a4c199af58e.office.live.com/view.aspx/401%20Error%20Code/401%20Error%20Code.docx)

To add custom error pages in different languages, just add the pages to the appropriate local language folder.  You can find the local language folder information here...  [http://msdn.microsoft.com/en-us/library/bb266177.aspx](http://msdn.microsoft.com/en-us/library/bb266177.aspx)
