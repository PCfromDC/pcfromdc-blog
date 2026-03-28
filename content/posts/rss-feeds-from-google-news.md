---
title: "RSS Feeds from Google News"
date: 2011-03-24T14:07:00+0000
categories: ["SharePoint"]
tags: ["SharePoint 2010", "Google", "RSS", "SharePoint 2007", "Web Parts"]
aliases:
  - "/2011/03/rss-feeds-from-google-news.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Sometimes the news sources you are trying to collect do not have good RSS feeds.  Especially if you are trying to collect information about a very niche subject.  Google has made it very easy to collect news to display as an RSS feed within SharePoint.

1)  Go to http://news.google.com 

2)  Type in your search parameters

3)  Modify the results URL by adding "&output=rss" at the end of the URL, and go to the new URL

4)  Verify that you are indeed getting the feed.

[![](/images/XSL Feed.jpg)](/images/XSL Feed.jpg)

4)  Add the RSS Viewer webpart to your page

5)  Under RSS Properties, in the RSS Feed URL, add the modified URL.

[![](/images/URL Entry.jpg)](/images/URL Entry.jpg)
