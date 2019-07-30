---
title: "NHibernate One to One mapping"
description: "After a lot of trial and error i finally found a good solution to my one-to-one mapping situation"
date: 2015-04-27
tags: ["NHibernate", "Fluent NHibernate"]
---

A working One to One mapping in NHibernate (using FluentNHibernate for this example):

```cs
public class NotificationMap : ClassMap<Notification>
{ 
    public NotificationMap() 
    { 
        Id(x => x.Id); 
        HasOne(x => x.Trigger).PropertyRef(x => x.Notification).Cascade.All();
    } 
}
```
```sql
CREATE TABLE [dbo].[Notification]( [Id] [uniqueidentifier] NOT NULL ) 
```
```cs
public class NotificationTriggerMap : ClassMap<NotificationTrigger> {
    public NotificationTriggerMap() 
    { 
        Id(x => x.Id); 
        References(x => x.Notification).Not.Nullable(); 
    } 
}
```
```sql
CREATE TABLE [dbo].[NotificationTrigger]( 
    [Id] [uniqueidentifier] NOT NULL, 
    [NotificationId] [uniqueidentifier] NOT NULL ) 

ALTER TABLE [dbo].[NotificationTrigger] 
    WITH NOCHECK ADD CONSTRAINT [FK_NotificationTrigger_NotificationId_Notification_Id] 
    FOREIGN KEY([NotificationId]) 
    REFERENCES [dbo].[Notification] ([Id])
```
```cs
var notification = new Notification();
var trigger = new NotificationTrigger { Notification = notification };
notification.Trigger = trigger;
session.Save(notification);
session.Flush(); //Will create both entities in the database
```

## Resources
- [Correct answer at StackOverflow](http://stackoverflow.com/questions/643656/fluent-nhibernate-hasone-withforeignkey-not-working#answer-2386399)