---
title: "Programatically Create a SQL Table with a Variable in The Name"
date: 2011-02-19T17:47:00+0000
categories: ["SQL"]
aliases:
  - "/2011/02/programatically-create-a-sql-table.html"
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I ran into a situation where I needed to compare an Excel table to an existing SQL table, then update the SQL table with only the missing information.  My goal was to create a temporary table, then compare and update.  The problem for me arises when more than 1 user wants to update their information at once; I am worried that using a static name for a temp table might create issues.  Here is how to create and delete a table with a variable in the name.

declare @makeTable nvarchar(max)declare @delTable nvarchar(max)declare @tablename varchar(200)declare @variable varchar(20)

set @variable= 'userName' set @tablename = 'Temp_ProjectRisksExcel_' + @variable

-- Create Tabele

set @makeTable = 'create table ' + @tablename +       '(          [ExecutionPlanCode] [varchar](20) NULL,      [RiskIdentifierCode] [varchar](40) NULL,      [RiskTitle] [varchar](250) NULL,      [RiskDescription] [varchar](1500) NULL,      [Occurrence] [varchar](50) NULL,      [Impact] [varchar](50) NULL,      [RiskConsequence] [varchar](1500) NULL,      [Strategy] [varchar](50) NULL,      [RiskMitigation] [varchar](1500) NULL,      [Comments] [varchar](1500) NULL,      [RiskRetired] [varchar](50) NULL,      [RiskRetiredComments] [varchar](1500) NULL,      [RiskRetiredDate] [date] NULL      )'      exec(@makeTable)

-- Delete Table

set @delTable = 'DROP TABLE ' + @tablename

exec(@delTable)
