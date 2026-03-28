---
title: "Get First and Last Day of Current Month in SQL"
date: 2012-03-03T03:06:00+0000
categories: ["SQL"]
tags: ["SSRS"]
aliases:
  - "/2012/03/get-first-and-last-day-of-current.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I have been spending most of the last couple of day hammering out reports in SSRS.  I needed to get information for the current month, but needed to know the first and last dates to set my query.  I do not want to run into any kind of Azure Date issues ([http://www.wired.com/wiredenterprise/2012/03/azure-leap-year-bug/](http://www.wired.com/wiredenterprise/2012/03/azure-leap-year-bug/))

declare @reportDate datetime 

declare @lastDate datetime 

set @reportDate = GETDATE() 

Set @reportDate = DateAdd(Day, 1, @reportDate - Day(@reportDate) + 1) -1 

Set @lastDate = DateAdd(Month, 1, @reportDate - Day(@reportDate) + 1) -1 

select @reportDate, @lastDate

 Update (05/01/2012):

I was not very happy with the above query, so I have updated it...  I have also added the functionality to set the time back to midnight...

```
declare @first datetime
declare @last datetime
set @first = dateadd(day, 1, getdate() - day(getdate()))
set @last = dateadd(day, -1, dateadd(month, 1, @first))
select @first, @last
set @first = DATEADD(dd, DATEDIFF(dd, 0, @first), 0)
set @last = DATEADD(dd, DATEDIFF(dd, 0, @last), 0)
select @first, @last

```

[![](/images/FirstLastDayResults.jpg)](/images/FirstLastDayResults.jpg)
