---
title: "When did i boot my computer?"
description: "I use a time logger at work, but alot of times i forget what time i started, so to get the computer boot time, i have created a little powershell function"
date: 2016-02-29
tags: ["Powershell"]
---
I use a time logger at work, but alot of times i forget what time i started, so to get the computer boot time, i have created a little powershell function i have added to my profile:
```powershell
function boottime 
{ 
  $bootTime = (Get-WmiObject Win32_OperatingSystem).LastBootUpTime
  [System.Management.ManagementDateTimeConverter]::ToDateTime($bootTime) 
}
```