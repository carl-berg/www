---
title: "Query columns in a database"
description: "This is a trick to find all columns of a specific type in a database."
date: 2012-09-11
tags: ["Sql"]
---

I just found this trick for finding all columns of a specific type in a database. Nothing for every day use, but when you need it, this is pretty powerful.

```sql
SELECT 
	OBJECT_NAME(SYS.COLUMNS.OBJECT_ID) AS TableName
	,SYS.COLUMNS.NAME AS ColumnName
FROM SYS.COLUMNS
JOIN SYS.TYPES ON SYS.COLUMNS.USER_TYPE_ID = SYS.TYPES.USER_TYPE_ID
WHERE SYS.TYPES.NAME = 'Ntext'
ORDER BY SYS.COLUMNS.OBJECT_ID
```

This example fetches all columns of type `NTEXT` in the database.

**Update: The above version does not seem to work on SQL Server 2000, however the below version seems to work on SQL Server 2000 and upwards.**

```sql
SELECT 
	sysobjects.name AS TableName
	,syscolumns.name AS ColumnName
FROM sysobjects 
	INNER JOIN syscolumns ON sysobjects.id = syscolumns.id 
	INNER JOIN systypes on syscolumns.xtype=systypes.xtype 
WHERE sysobjects.xtype='U' 
	AND systypes.name = N'ntext'
ORDER BY sysobjects.name, syscolumns.colid
```

This version allows backwards compatibility with SQL Server 2000

## Resources
- <http://blog.sqlauthority.com/2007/08/09/sql-server-2005-list-all-the-column-with-specific-data-types>