---
title: "Create a Data Grid View Using SPD 2007 and a SQL Stored Procedure"
date: 2011-03-02T03:04:00+0000
categories: ["SQL"]
tags: ["Custom Page", "Connections", "SharePoint Designer"]
aliases:
  - "/2011/03/create-a-data-grid-view-using-spd-2007.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

Let's say that we have a SQL table of Resources (people), and in another SQL table we have their schedules.  How can we view a list of people who are available to work certain dates?  Basically, we want the user to enter a "Start Date" and an "End Date" and retrieve a Data Grid list of all people in the company who are available to work within those dates.

1) Create the sample databases and data.    [Get SQL script here](http://public.bay.livefilestore.com/y1pZ-OHzNTV9IOYIDMDFKdmTEQtZY4N2v1J1gOleaOWIhKcfQhigPhxWcF5nI_JhxaWfl3i4UeKB6gjhd9Pht_lyw/DemoTables.sql?download&psid=1)

2) Create the Stored Procedure

[![](/images/Stored Procedure.jpg)](/images/Stored Procedure.jpg)[Get Stored Procedure script here](http://hrrqga.bay.livefilestore.com/y1pJEVN3wU4IwDrCu_yWprjENFP8WBHgOQjvASD-nmMOrdayWi1ybzf1wzpclff7y5un5XgJhJOBNQTG5DyfC5Nv_0Ul3SVRhu3/sp_GetAvailableResources.sql?download&psid=1)

3) Next, we will open up SharePoint Designer 2007, and go to the top level of the site.  For example, http://pc2007.local.  We are now going to create a blank page based off the default,master page.http://pc2007.local  > _catalogs > masterpage > default.master.  Right click the default.master and select "New from Master Page".  This will open up a blank web page.

[![](/images/Select Default Master.jpg)](/images/Select Default Master.jpg) [![](/images/Select New From Master.jpg)](/images/Select New From Master.jpg)4) To incorporate out own ideas, we will need to go to the Common Content Tasks for PlaceHolderMain and select Create Custom Content.[![](/images/Create Custom Content.jpg)](/images/Create Custom Content.jpg)5) Within the PlaceHolderMain, I am going to add a 4x4 table.  Table > Insert Table 

6) I am going to label my columns and insert 2 Calendar Date Pickers.

[![](/images/Calanders Inserted.jpg)](/images/Calanders Inserted.jpg)7) We now want to change the ids from "Calendar1" to "indate" and from "Calendar2" to "enddate"

[![](/images/change calendar names.jpg)](/images/change calendar names.jpg)8) Within the Data Source Library tab, click "Connect to a database..."

[![](/images/Database Connection.jpg)](/images/Database Connection.jpg)

9) Enter your SQL server information, click Next.  And you will receive a warning about passwords being saved as plain text.

10) Select the database where your tables are stored, and we are going to select the Stored Procedure radial button.

[![](/images/Select Database.jpg)](/images/Select Database.jpg)11) Click Finish!  A new window should pop-up to Edit Custom SQL Commands.

12) Under the Select tab, select the Stored Procedure radial button (again) and select the appropriate stored procedure, and click the "OK" button.

13) Select the "General Tab" and give your database connection a useful name... and save it!

14) This is just my preference, but I will select the bottom left cell of my table to insert my source control...

[![](/images/Source Control.jpg)](/images/Source Control.jpg)15) Hover your mouse over the newly created database connection and select "Insert Data Source Control"  A Refresh Data Source Schema will pop-up, and you will want to click the "Ok" button.

[![](/images/Insert Data Source Control.jpg)](/images/Insert Data Source Control.jpg)16) You will want to open the Common SqlDataSource Tasks and select "Configure Data Source..."

[![](/images/Select Configure Data Source.jpg)](/images/Select Configure Data Source.jpg)17) Click Next as we have already created the connection

18) However, we want to save the connection string locally, so give it a friendly name, and click Next.

[![](/images/Save Connection String.jpg)](/images/Save Connection String.jpg)19) Verify the "Specify a custom SQL statement of stored procedure" radial button is selected, and click Next.

20)  Under the Select tab, select the Stored Procedure radial button, find your Stored Procedure, and click Next.

[![](/images/Select Stored Procedure.jpg)](/images/Select Stored Procedure.jpg)21)  We now want to tell the connection what variables we want to pass to the Stored Procedure.  Our sources are both "Control" types.  However for the "enddate", make sure that your ControlID is the name of the control that has the End Date...  We had named the calendar "enddate".

22)  Set up the In Date control and press the Next button, and then the Finish button.  You will get a warning about where the configuration for the connection is being saved.

[![](/images/Controls Selected.jpg)](/images/Controls Selected.jpg)23)  Merge the rows under the calendar.

[![](/images/Merge Cells.jpg)](/images/Merge Cells.jpg)24) Under the Toolbox tab, we are going to expand the Data Section, grab the GridView control and drop it into the row under the calendar...  The one where we just merged the cells...

[![](/images/Datagrid View.jpg)](/images/Datagrid View.jpg)25) Open up the Common GridView Tasks and under the Choose Data Source, select your Data Source.

[![](/images/Common GridView Tasks.jpg)](/images/Common GridView Tasks.jpg)26)  Under the Common GridView Tasks, enable Paging and Enable Sorting.

[![](/images/Enable Sorting and Paging.jpg)](/images/Enable Sorting and Paging.jpg)27)  Save your work, check in your document, navigate to your new page, select an In and Out Date, and look at who is available!

[![](/images/My Finished Product.jpg)](/images/My Finished Product.jpg)
