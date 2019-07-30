---
title: "ILMerge"
description: "Trying out ILMerge"
date: 2012-09-18
tags: ["ILMerge"]
---

I recently tried out ILMerge in order to merge a few assemblies into a single executable console application. 

```powershell
ILMerge.exe /out:MergedApp.exe App.exe AnAssembly.dll AnotherAssembly.dll
```

This makes it easier to deploy an application as a stand alone file. I don't think i would bother using this for a web application, but it's pretty handy for a forms application or a console application. 

I would recommend getting ILMerge through nuget, but it can also be downloaded through chocolately or as an msi file.

## Resources
- [Reference](http://research.microsoft.com/en-us/people/mbarnett/ilmerge.aspx)
- [Nuget](https://nuget.org/packages/ilmerge)
- [MSI Download](http://www.microsoft.com/en-us/download/details.aspx?id=17630)
- [Chocolately](http://nuget.org/packages/chocolatey)

