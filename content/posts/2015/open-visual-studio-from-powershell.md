---
title: "Open visual studio from powershell"
description: "A neat shortcut to open your solution from powershell"
date: 2015-01-11
tags: ["Powershell"]
---

If you are like me, you spend some time working with your source control from the command line (powershell in my case). Often after i have navigated to a folder and synced in changes i want to open the solution contained somewhere below the folder i am currently in.

This function will open up the first available solution file under your current directory in visual studio
```powershell
function sln 
{ 
    $file = Get-ChildItem -recurse *.sln | Select-Object -First 1
    if($file)
    {
        start -FilePath $file.FullName
    }
}
```
So stick this snippet in your powershell profile and be on your merry way!

## Resources
- [Start-Process](http://technet.microsoft.com/en-us/library/hh849848.aspx)
- [Create your own powershell functions](http://windowsitpro.com/windows/create-your-own-powershell-functions)