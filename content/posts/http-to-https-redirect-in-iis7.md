---
title: "Http to Https Redirect in IIS7"
date: 2011-02-10T20:31:00+0000
tags: ["Custom Page", "Redirecting", "SSL", "IIS 7"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

To keep with the subject of error pages, I thought it would be good to touch on how to redirect end users and force them to use port 443 instead of port 80.  There is a very simple way to accomplish this in IIS7, using the same Error Pages Feature that we used in the last posting on creating a [SharePoint Custom Login Error (401) Page](http://pcfromdc.blogspot.com/2011/02/sharepoint-custom-login-error-401-page.html). 

1) Go to IIS and select your web site.

2) If you have not already done so, edit the site bindings to add the port 443

3) Open the Error Pages Feature under the IIS area.  In the right column, click Add.[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgCB_fePkTi9_mw1qMAou7FJ-n-rOMU81u1uSA-mI5hSNZ3adia2MVIH61Xs70MF-vF26rcGvahRliVs397iTgMtzZLJCaA9zuU0_OBhPOyFL-eS1otYrRQ2Jbb5kHg3kZ9FJfqXK0aUCUL/s640/Add+Error+Page.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgCB_fePkTi9_mw1qMAou7FJ-n-rOMU81u1uSA-mI5hSNZ3adia2MVIH61Xs70MF-vF26rcGvahRliVs397iTgMtzZLJCaA9zuU0_OBhPOyFL-eS1otYrRQ2Jbb5kHg3kZ9FJfqXK0aUCUL/s1600/Add+Error+Page.jpg)

4) Add a Status Code of 403.4 and select Respond with a 302 redirect.  Put in YOUR https address![![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjCpklqYvVqTzba03-2zKXWvy_BcBzp4j707z6tgnPzjlp2pwFB-2gbT-nNAQOYyCP-Jt7oTV00jGqAS1jAUiZrYRhwkxnEMwwpdIVvhz1qNNxxUDW5wqFQACCS5vlinLRci88hXsRr-U8_/s400/Create+custom+error+page.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjCpklqYvVqTzba03-2zKXWvy_BcBzp4j707z6tgnPzjlp2pwFB-2gbT-nNAQOYyCP-Jt7oTV00jGqAS1jAUiZrYRhwkxnEMwwpdIVvhz1qNNxxUDW5wqFQACCS5vlinLRci88hXsRr-U8_/s1600/Create+custom+error+page.jpg)

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-UdY2RjT1pcm14Hd8nJ8PYt2REwsW8KF2fMW7pzEhJdNHCAsbcmqfmIrCUpjnT3i9w9ywry_OtM2-RzXEhmt2j4DNo66LJfawks2lGRcm9S8XewzSFAn6ctK9KCkk1L5Ld5OSsnvKlanE/s640/Should+look+like.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-UdY2RjT1pcm14Hd8nJ8PYt2REwsW8KF2fMW7pzEhJdNHCAsbcmqfmIrCUpjnT3i9w9ywry_OtM2-RzXEhmt2j4DNo66LJfawks2lGRcm9S8XewzSFAn6ctK9KCkk1L5Ld5OSsnvKlanE/s1600/Should+look+like.jpg)

5) Open the SSL Settings in the IIS area for your site.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgu7wbQRSPpqEOtb3NB3IIAQpZZbLAu5ezakoNrBTvi8sdOtvIw08aOFwaeFA3MBgCTDROMfjJQ3vssGe49jeP4d4u3W68Jg8lBYVBYnW5jmpFRyL6oivydhQL0EjaTg0mHwYqV24lWX30j/s640/SSL+Settings+Selected.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgu7wbQRSPpqEOtb3NB3IIAQpZZbLAu5ezakoNrBTvi8sdOtvIw08aOFwaeFA3MBgCTDROMfjJQ3vssGe49jeP4d4u3W68Jg8lBYVBYnW5jmpFRyL6oivydhQL0EjaTg0mHwYqV24lWX30j/s1600/SSL+Settings+Selected.jpg)

6) Click the Require SSL check box, and click Apply in the upper right Actions column.[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjX9JcHKe4I-rp6p1R5WSCeyDF4yEsW9k19d_cPFX5yT0R2FLFjQSd7lMg3kS04-yfLViDIRfCMuTqAS3xw4IUW_mI2pA6J8Y4sn1lVYEm_usAFv83ZM0Mn9JG5OzOsP1Sq5lDMEE5MucK/s640/Force+SSL.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjjX9JcHKe4I-rp6p1R5WSCeyDF4yEsW9k19d_cPFX5yT0R2FLFjQSd7lMg3kS04-yfLViDIRfCMuTqAS3xw4IUW_mI2pA6J8Y4sn1lVYEm_usAFv83ZM0Mn9JG5OzOsP1Sq5lDMEE5MucK/s1600/Force+SSL.jpg)
