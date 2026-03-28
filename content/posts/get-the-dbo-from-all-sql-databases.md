---
title: "Get The DBO From All SQL Databases"
date: 2012-01-17T03:55:00+0000
categories: ["SQL"]
tags: ["DBO"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I have finally decided to start keeping track of the useful SQL commands that I have used.  Mostly because I am tired of rewriting them.  Also, if I have had to use them, then I sure that someone else (or me again) might find them useful.

While moving databases around within SQL to optimize IOPS and/or drive utilization, you might have a need to put the Database Owner back to what it was originally. Before you drop your databases, take a look at the DBO first.

This will grab all the dbo's of all the databases on your server:

```
select SUSER_SNAME(owner_sid) as username, name from sys.databases

```

Now, if you want to change the DBO...

```
sp_changeDbOwner @loginame = 'domain\username'

```

However, you might run into an error is the DBO is already a user or aliased in the database.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiUY4R8JkQeYnVwGxDRAj_NqdMMrCDgNLRnloaW9xx7BengFotDzCGy8t_9Irdkju52rlQ5mSheH3GsZ8kEa9OLpW6KyqrcD26Z4YjrS6_trJdGUBPB_2Zmqw6f4z56EjrlSiCkotUsP1qt/s1600/Error+Message.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiUY4R8JkQeYnVwGxDRAj_NqdMMrCDgNLRnloaW9xx7BengFotDzCGy8t_9Irdkju52rlQ5mSheH3GsZ8kEa9OLpW6KyqrcD26Z4YjrS6_trJdGUBPB_2Zmqw6f4z56EjrlSiCkotUsP1qt/s1600/Error+Message.jpg)
To fix this problem, run the following:

```
USE 
GO
SP_DROPUSER 'domain\username'
GO
SP_CHANGEDBOWNER 'domain\username'

```

UPDATE 02/04/2015

Added drop user and change owner code.
