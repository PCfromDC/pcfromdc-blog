---
title: "Weather RSS Data View"
date: 2010-04-19T19:48:00+0000
aliases:
  - "/2010/04/weather-rss-data-view.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

The company that I work for travels people all over the word every week of the year. I thought that it would be nice to see what the weather will be so they can be better prepared for their job.

1) Create a Custom List within your SharePoint site, with columns Venue Name, Address, City, State, Zip, and Country.

2) Open up the site in SharePoint Designer.

3) Add a DataView Web Part to bring the Venue Address to the Web Site.

4) Open the Data Source Library

5) Create a Server-Side Script

![](/images/1.jpg)
6) Enter the URL information...

[http://weather.msn.com/rss.aspx?wealocations=21076&weadegreetype=F](http://weather.msn.com/rss.aspx?wealocations=21076&weadegreetype=F)

You can change the "21076" to your own default zip code.

The "add/modify paramiters" should have automatically filled in.

7) Drag the Data Source where you want it displayed on your web site.

8) Hover over the right edge of the Weather Script that you just added and select the Common Tasks button

a) Select Edit Columns

b) Remove all columns except for the description column

c) Select the Common xsl:value-of Tasks

d) Set the Data Field (description)

e) Set the Format as Rich Text (you will get a warning)

9) Go back to the Data View Web Part for the "Venue" that you created in Step 3 and hover over the right corner to select th eCommon Data View Tasks --> Web Part Connections

10) Click Add...

![](/images/2.bmp)

![](/images/5.bmp)
![](/images/3.bmp)

11) Click Next and then Finish...
