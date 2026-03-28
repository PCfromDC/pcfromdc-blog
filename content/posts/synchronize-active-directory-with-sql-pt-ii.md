---
title: "Synchronize Active Directory With SQL pt II"
date: 2013-11-27T19:36:00+0000
categories: ["PowerShell", "SQL"]
tags: ["SQL 2012", "Synchronization", "Active Directory"]
aliases:
  - "/2013/11/synchronize-active-directory-with-sql.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

One of the big complaints about querying Active Directory from within SQL is the lack of ability to easily return more that 1,000 items. In this post, we are going to solve that dilemma by throwing in some PowerShell to get around the Page Size limit. In this blog, I will demonstrate how to take 28,000+ users from Active Directory, put them into a SQL Table, and update the Production SQL Table. I will also go through some of the lessons learned from this experience, because it was not as easy as I had originally had hoped. Why synchronize AD to SQL? From a user manageability perspective, I think this is a great way to allow users to maintain their own information, such as their Address and Phone Numbers, or any corporate information without bothering HR or the IT staff to update AD profiles.

Objective

The objective of this post is to take all users from Active Directory, along with their lastLogon times, and synchronize them into a SQL table. This is going to be accomplished through the use of PowerShell and SQL.

Requirements

I have created this in PowerShell ISE version 3.0 on Windows Server 2012 and SQL Server 2012. The person running the script should have permissions to create and write into SQL, and read items from AD. You will need to have the SQLPS module and the ActiveDirectory modules installed. The SQL module should be installed when you install SSMS, and you can add the ActiveDirectory module by activating the feature through the Add Roles and Features Wizard:

[*](/images/Add AD Feature.png)
You should also have a database and a table in place to save all of your Production User Data. I created a database name "pcDemo_Personnel" and this is the table that I will  use for this post:

```
USE [pcDemo_Personnel]
GO
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[pcDemo_SystemUsers](
     [RowID] [int] IDENTITY(1,1) NOT NULL,
     [UserLogin] [varchar](500) NOT NULL, 
     [Password] [varchar](200) NULL,
     [UserFullName] [varchar](500) NULL,
     [UserLastName] [varchar](1000) NULL,
     [UserFirstName] [varchar](1000) NULL,
     [UserCompany] [varchar](200) NULL,
     [UserOfficeLocation] [varchar](200) NULL,
     [Department] [varchar](50) NULL,
     [UserTitle] [varchar](500) NULL,
     [Manager] [varchar](500) NULL,
     [UserPhone] [varchar](200) NULL,
     [UserEmail] [varchar](150) NULL,
     [CreatedDate] [datetime] NULL,
     [System_Role] [varchar](50) NULL,
     [ReadOnly] [varchar](1) NULL,
     [lastLogon] [datetime] NULL,
     [userAccountControl] [varchar](50) NULL,
     [TwitterHandle] [varchar](50) NULL,
     [BlogAddress] [varchar](500) NULL,
     [FacebookPage] [varchar](500) NULL,
     [SSN] [varchar](12) NULL
) ON [PRIMARY]
GO
SET ANSI_PADDING OFF
GO
ALTER TABLE [dbo].[pcDemo_SystemUsers] ADD  CONSTRAINT [DF_SystemUsers_CreatedDate]  DEFAULT (getdate()) FOR [CreatedDate]
GO

```

Getting User Information from AD
To query Active Directory, we will be using the [Get-ADUser cmdlet](http://technet.microsoft.com/en-us/library/ee617241.aspx), and as we learned from my previous post, [Sync Active Direcory to SQL](/posts/sync-active-directory-to-sql_17/), lastLogon time is stored on each Domain Controller so we will need to query each server to get each person's lastLogon time. We will query each server by creating an array of Domain Controller server names, and query each server in the array. For example, to query the first server in the array, you would use the following command which will return all properties for each user and put the returned data into the Variable $Users:

```
$Users = Get-ADUser -Filter * -Server $OUs[0] -Properties *
```

We will go through the -Filter and -Properties parameters later in this post.

Do not take querying your servers lightly. You should talk to your AD team BEFORE querying your domain controllers, and see if they have a preferred server that they wish you to query and/or specific times that they wish you to run your queries. Obviously, if they do not want you to query each server, then you will not be able to get accurate lastLogon times. Also, this information could be important to them as a health check of their servers.

Now that you have your User Information, we will be manipulating the information and passing it into SQL.

Putting User Information into SQL (Attempt #1)

As in the previous post, I started by creating a table in SQL called Temp_ADUsers.  Since we are now able to write a SQL query within PowerShell to insert information into SQL, I figured I would just loop through each individual user and insert them and their information into my table. Then I would query the second OU and update the table with each user's lastLogon time, and then the final server.

The SQL query looked like this (we will talk about the setLastLogon Function shortly):

```
foreach ($user in $Users)
{
   if ($user.SamAccountName -ne "Administrator" -and $user.SamAccountName -ne "Guest" -and $user.DisplayName.Length -gt 1)
   {
      $sAMAccountName = $user.sAMAccountName
      $firstName = $user.GivenName
      $lastName = $user.Surname
      $displayName = $user.displayName
      $company = $user.company
      $department = $user.department
      $title = $user.title
      $manager = $user.manager
      $telephoneNumber = $user.telephoneNumber
      $mail = $user.mail
      $userAccountControl = $user.userAccountControl
      $lastLogon = setLastLogon($user.lastLogon)
      $query1 = "INSERT INTO [dbo].[Temp_ADUsers]
      (
         [UserLogin]
        ,[FirstName]
        ,[LastName]
        ,[DisplayName]
        ,[UserAffiliation]
        ,[UserOrganization]
        ,[UserTitle]
        ,[Manager]
        ,[UserPhone]
        ,[UserEmail]
        ,[userAccountControl]
        ,[DC01_lastLogon_Raw]
      )
      VALUES
      (
         '$sAMAccountName'
        ,'$firstName'
        ,'$lastName'
        ,'$displayName'
        ,'$company'
        ,'$department'
        ,'$title'
        ,'$manager'
        ,'$telephoneNumber'
        ,'$mail'
        ,$userAccountControl
        ,'$lastLogon'
       )"
       Invoke-Sqlcmd -query $query1 -database "pcDemo_Personnel" -ServerInstance "sql2012-03"
   }
}
```

Within the script, I put some time tracking in an Out-File so that I could track the Get-ADUser query time (proof for the AD Team if needed), as well as the time it takes to insert/update SQL. My times returned were pretty horrible...

[![](/images/Slow Query.png)](/images/Slow Query.png)
This method of running the insert and update SQL queries also brought up another issue:

[![](/images/1- Subscription(1).png'Connor+Error.png)](/images/1- Subscription(1).png'Connor+Error.png)
Because there are people in Active Directory with single apostrophes in their name like Mr. O'Connor, I would have to come up with a work around for any property value that has an apostrophe in it. Luckily, the time to completion is so horrible that I figured solving for an apostrophe should be pretty simple and will address this issue at a later date.

If you recall, another issue with dealing with the lastLogin property is that the lastLogon time is "number of 100 nanosecond intervals since January 1, 1601 (UTC)" To solve this issue, I created a function with a foreach to take care of cleaning up the lastLogon time and converting it to a dateTime in UTC (Zulu):

```
foreach ($user in $users)
{
    if (!$user.lastLogon)
       {
          $user.lastLogon = 0
       }
    else
       {
          $user.lastLogon = [datetime]::FromFileTime($user.lastLogon).ToString('u')
       }
}
```

Putting User Information into SQL (Attempt #2)

The short lived Attempt #2 was based off a multiple item insert so that I could get rid of the foreach loop. It was a short lived idea because you can only bulk insert up to 1,000 items into SQL. Then I started looking into SQL's [Bulk Insert Query](http://technet.microsoft.com/en-us/library/ms188365.aspx), which led me to a question posed in [StackOverflow](http://stackoverflow.com/questions/5736103/create-table-with-records-in-sql-server-with-powershell-whats-best), which led me to the [Hey, Scripting Guy! Blog](http://blogs.technet.com/b/heyscriptingguy/archive/2010/11/01/use-powershell-to-collect-server-data-and-write-to-sql.aspx) titled Use PowerShell to Collect Server Data and Write to SQL. *After reading both articles, it became evident to me that every Server or Network Administrator now has the ability to take Enterprise Information and dump it into SQL. In my opinion, THIS IS HUGE!!!  Thank you again Chad Miller ([@cmille19](https://twitter.com/cmille19)) for creating and sharing your SQL Functions!

Putting User Information into SQL (Attempt #3)

After reading through both blog posts mentioned above, I had an understanding of what needed to be done. I added the new functions to my script and commented out the foreach lastLogon cleanup loop to pass just raw data into the DataTable and into SQL. I gave it a run, and ran into an issue:

[![](/images/Table Column Errors.png)](/images/Table Column Errors.png)
Taking a look at the errors, the columns not belonging in the table are found in the returned properties of the Get-ADUser cmdlet. The current query looks like this:

```
# Get AD Properties
$properties = ("sAMAccountName", "displayName", "mail", "telephoneNumber", "physicalDeliveryOfficeName", "department", "userAccountControl", "company", "title", "lastLogon", "manager", "givenName", "Surname")
# Get Users
$users = $null
$users = Get-ADUser -Filter * -Server $ouServer -Properties (foreach{$properties})
```

To get around this error, I piped the output into a new table:

```
$users = Get-ADUser -Filter * -Server $ouServer -Properties (foreach{$properties}) | Select (foreach{$properties})
```

Ran the script again and received a new error:

[![](/images/lastLogon Null Error.png)](/images/lastLogon Null Error.png)
I un-commented out the foreach lastLogon cleanup loop and tried again. Things seemed to be running, so off I went for coffee, and when I returned, the script had run its course... cleanly. SQL looked like this:

[![](/images/SQL Finished.png)](/images/SQL Finished.png)
Now it did not take me 3 hours to drink my coffee, so lets look at the actual run time:

[![](/images/Fast Query.png)](/images/Fast Query.png)
Holy COW look at those numbers! This process saved 3 hours, 4 minutes, and 18 seconds!

Now that we are able to load User data into SQL, it is time to put the rest of the script together, like Drop the Temp Tables, Set the Get-ADUser -Filter, and move the data from the Temp Tables into the Master AD Users Table.

Dropping Existing SQL Tables

Because the functions that create the SQL data tables do not automatically check to see if the tables exist before creation, it is necessary to delete existing tables so that we can import the data from AD.

```
# Drop table if it exists
$query1 = "IF OBJECT_ID('dbo.$tableName', 'U') IS NOT NULL DROP TABLE dbo.$tableName"
Invoke-Sqlcmd -Query $query1 -Database $databaseName -ServerInstance $dbServer 
```

Filtering Out Unwanted Users

There are accounts within AD that you probably do not want to surface, synchronize, and expose their properties, such as the Administrator account, Service accounts, and the krbtgt account. The quickest way is to use the Filter parameter. Typically these accounts do not have displayNames, so filtering out empty values should return People. This may not be true for your organization, however, your organization should have some sort of governance over the naming structure of service accounts. My Get-ADUser cmd looks like this:

```
$users = Get-ADUser -Filter {displayName -like "*"} -Server $ouServer -Properties (foreach{$properties}) | Select (foreach{$properties})
```

Clean SQL Table from the 1st Domain Controller 

Since I had to put a zero in for null values for the lastLogon time, it is necessary to clean up the table. Luckily it is pretty easy to set all 0 values back to null.

```
$query2 = "UPDATE [dbo].$tableName SET lastLogon = NULL WHERE lastLogon = '0'"
Invoke-Sqlcmd -Query $query2 -Database $databaseName -ServerInstance $dbServer
```

Update Current Users in Production Table

Now that all data from all Domain Controllers has been returned, and placed into Temporary SQL tables, we can update the users' information in the Production Table:

```
$tempTableName = "temp_" + $OUs[0] + "_Table"
$query11 = "UPDATE active
            SET
               active.UserLogin = LOWER(temp.sAMAccountName),
               active.UserFullName = temp.displayName,
               active.UserLastName = temp.Surname,
               active.UserFirstName = temp.givenName,
               active.UserCompany = temp.company,
               active.UserOfficeLocation = temp.physicalDeliveryOfficeName
               active.UserTitle = temp.title
               active.Manager = temp.manager,
               active.UserPhone = temp.telephoneNumber,
               active.UserEmail = temp.mail,
               active.lastLogon = CONVERT(DATETIME, temp.lastLogon),
               active.userAccountControl = temp.userAccountControl
               active.Department = temp.department
            inner join " + $tempTableName + " temp
               on active.UserLogin = temp.sAMAccountName
            WHERE LOWER(active.UserLogin) = LOWER(temp.sAMAccountName)
Invoke-Sqlcmd -Query $query11 -Database $databaseName -ServerInstance $dbServer
```

Inserting New Users in Production Table

After updating current users, it it time to insert new users into the production table. I run the Update query first to save time, granted only by the number of new users  to be inserted, but time is time. This is how I add any new users into the Production Table:

```
$query12 = "INSERT INTO [" + $databaseName + "].[dbo].[" + $activeTableName + "]
(
   [UserLogin],
   [UserFullName],
   [UserLastName],
   [UserFirstName],
   [UserCompany],
   [UserOfficeLocation],
   [Department],
   [UserTitle],
   [Manager],
   [UserPhone],
   [UserEmail],
   [System_Role],
   [ReadOnly],
   [lastLogon],
   [userAccountControl]
)
   SELECTLOWER(sAMAccountName),
   [displayName],
   [givenName],
   [Surname],
   [company],
   [physicalDeliveryOfficeName],
   [department],
   [title],
   [manager],
   [telephoneNumber],
   [mail]
   [System_Role] = 'User',
   [ReadOnly] = 'Y',
   CONVERT(DATETIME, [lastLogon]),
   [userAccountControl]
FROM " + $tempTableName + " AS temp
WHERE sAMAccountName <> '' and not exists
(
   SELECT LOWER(UserLogin)
   FROM " + $activeTableName + " AS active
   WHERE LOWER(active.UserLogin) = LOWER(temp.sAMAccountName)
)"
Invoke-Sqlcmd -Query $query12 -Database $databaseName -ServerInstance $dbServer
```

Getting lastLogon From More Than 1 Domain Controller

For each Domain Controller, we are going to create a column in a new table called temp_lastLogonTimes to store the lastLogon times for each user, then we update the columns with that information:

```
foreach ($OU in $OUs)
{
   # Create OU Columns
   $columnName = $OU + "_lastLogon"
   $query5 = "ALTER TABLE temp_lastLogonTimes ADD " + $columnName + " varchar(1000)"
   Invoke-Sqlcmd -Query $query5 -Database $databaseName -ServerInstance $dbServer
}
# Insert and Update Times Into Temp Table
$counter = 0
foreach ($OU in $OUs)
{
   if ($counter -lt 1)
      {
      # Insert Names and Times
      $query6 = "INSERT INTO [dbo].[temp_lastLogonTimes]
                   ([sAMAccountName]
                    [" + $OU + "_lastLogon])
                 Select
                     sAMAccountName
                    ,lastLogon
                 FROM
                     temp_" + $OU + "_Table"
      Invoke-Sqlcmd -Query $query6 -Database $databaseName -ServerInstance $dbServer
      }
   # Update OU lastLogon Times
   $query7 = "UPDATE [dbo].[temp_lastLogonTimes]
              SET " + $OU + "_lastLogon = lastLogon
              FROM temp_" + $OU + "_Table
              WHERE temp_lastLogonTimes.sAMAccountName = temp_" + $OU + "_Table.sAMAccountName"
   Invoke-Sqlcmd -Query $query7 -Database $databaseName -ServerInstance $dbServer
   $counter ++
}
```

Now that the temp_lastLogonTimes table has the lastLogon times for each user from each Domain Controller, it is time to compare the times and find the true lastLogon time. To accomplish this in PowerShell, we will query the new temp_lastLogonTimes table, add a column for the lastLogon time to the temp_lastLogonTimes datatable, compare values, and update the row.

```
# Get Table and Update Last Logon Value
$str_OUs = @()
foreach ($OU in $OUs)
{
   $str_OUs += "ISNULL(" + $OU + "_lastLogon, 0) as " + $OU + "_lastLogon"
}
$str_OUs = $str_OUs -join ", "
$query8 = "SELECT sAMAccountName, " + $str_OUs + " from temp_lastLogonTimes"
$arrayLLT = @()
$arrayLLT = Invoke-Sqlcmd -Query $query8 -Database $databaseName -ServerInstance $dbServer
$arrayLLT | Add-Member -MemberType NoteProperty -Name "lastLogon" -Value ""
$arrayLength = $arrayLLT[0].Table.Columns.Count - 1
$counter = 0
foreach ($sAM in $arrayLLT.sAMAccountName
{
   $max = $arrayLLT[$counter][1..$arrayLength] | Measure -Maximum
   $arrayLLT[$counter].lastLogon = $max.Maximum
   $counter ++
}
```

Updating lastLogon Time in Production Table

Like before, we will take our datatable, create a final temp table in SQL, and move the data to the production table.

```
# Get DataTable
$arrayLLT = $arrayLLT | Select sAMAccountName, lastLogon
$dt2 = $arrayLLT | Out-DataTable
# Create SQL Table
Add-SqlTable -ServerInstance $dbServer -Database $databaseName -TableName $tableNameLLT -DataTable $dt2
# Write DataTable into SQL
Write-DataTable -ServerInstance $dbServer -Database $databaseName -TableName $tableNameLLT -Data $dt2
$query13 = "UPDATE [dbo].[" + $activeTableName + "]
            SET " + $activeTableName + ".lastLogon = temp_lastLogons.lastLogon
            FROM temp_lastLogons
            WHERE LOWER(temp_lastLogons.sAMAccountName) = LOWER(" + $activeTableName + ".UserLogin)"
Invoke-Sqlcmd -Query $query13 -Database $databaseName -ServerInstance $dbServer
```

Now that the solution is completed, let's look at the time to query 3 domain controllers:

[![](/images/Final 3 DC Query.png)](/images/Final 3 DC Query.png)
 And if we take a look at the single DC query:

[![](/images/Final 1 DC Query.png)](/images/Final 1 DC Query.png)
So based off the data above, there is a run time average of approximately 1.75 minutes per DC queried when dealing with 28782 users.

You can download the complete code from [GitHub:](https://raw.github.com/PCfromDC/AD2SQL/master/README.md)

Create The Timer Job

Now that we can synchronize all users from AD into SQL, we need to create a scheduled task to run our PowerShell script to keep SQL synchronized on a regular basis. Now, this can be run daily or every couple of hours pending on how your organization works.

Please review my post on [Creating a Scheduled Task With PowerShell](/posts/create-scheduled-task-with-powershell/).

However, what if the server Admin will not allow you to create a timer job using the Task Scheduler? Luckily, a colleague of mine, Don Kirham ([@DonKirkham](https://twitter.com/DonKirkham)) posed a similar question on our company's Yammer site. Don wanted to know if it was possible to use SQL Server Agent jobs to run the job instead. The answer is, yes you can, but it is not as straight forward as one would like. You can read how to run this the Sync AD to SQL script from a SQL Server Agent Job from my blog post called [Run a PowerShell v3 Script From a SQL Server Agent Job](/posts/run-powershell-v3-script-from-sql/)

UPDATES:

11/29/2013 Added link to Creating a Scheduled Task Blog, and added more kudos to Chad Miller for his contribution of the SQL Functions.

01/01/2014 Added link to blog post: Run a PowerShell v3 Script From a SQL Server Agent Job
