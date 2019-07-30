---
title: "Getting an AD group SID"
description: "Using powershell to get a SID for a user group in AD"
date: 2014-03-19
tags: ["Powershell", "Active Directory"]
---

Getting a SID for an AD-group is perhaps a little harder than it should be. You can see it in the UI, but you can't copy it only view it as hex, binary, decimal or ocatal which is pretty useless.

![AD group seen from UI](/img/ad-group-ui.png)

So as usual, powershell saves the day. Here's what to do:

On the AD-server, Import the ActiveDirectory module in powershell (if not already installed):
```powershell
Import-Module ActiveDirectory
```
Once that is done you can list groups like this:
```powershell
Get-ADGroup -Filter {Name -like "D*"} | Select Name, SID | Format-Table -Auto
```
![AD group seen from UI](/img/ad-group-powershell.png)

## Resources
- [TechNet](http://technet.microsoft.com/en-us/library/ee617196.aspx)
- [TechNet Forum](http://social.technet.microsoft.com/Forums/windowsserver/en-US/fb127470-fff3-4e81-afca-a414ad5840f5/how-to-find-usergroup-known-only-sid?forum=winserverDS)