---
title: "SemVer in practice"
description: "How to set assembly version according to semantic versioning (with release candidates) in .net assemblies"
date: 2014-01-14
tags: ["SemVer"]
---

I have been using semantic versioning when developing and releasing new versions of software for a while now.

Recently our team took a step towards using release candidates (during the stabilization before a live release) in order to determine which version is currently out in the test environment for testing and QA, and to allow testers to tag issues found with the actual version where the bug/issue was found.

According to SemVer we would like to tag versions like `0.1.0-rc.2` for example. So using the trial and error approach we adjusted the `AssemblyVersion`, compiled and we got exceptions. Ok... Next attempt, using `AssemblyFileVersion` works a little better, but still leaves us with compiler warnings that probably can be turned off somehow. Still leaves me not completely pleased. Third attempt using `AssemblyInformationalVersion` seems to be working fine, no compiler warnings, no errors.

## Assembly attribute
```cs
[assembly: AssemblyInformationalVersion("0.5.0-rc.2")]
```

Just one problem with this approach. We were currently using the `AssemblyFileVersion` to display the current version in the website footer and it wasn't showing the `AssemblyInformationalVersion` like i wanted it to, so here's how you can do just that.

## Access informational version
```cs
public static IHtmlString RenderProductVersion(this HtmlHelper helper)
{
    var assembly = helper.ViewContext.Controller.GetType().Assembly;
    var fileVersionInfo = FileVersionInfo.GetVersionInfo(assembly.Location);
    return new HtmlString(fileVersionInfo.ProductVersion);
}
```

## Resources
- [AssemblyInformationalVersionAttribute](http://msdn.microsoft.com/en-us/library/system.reflection.assemblyinformationalversionattribute(v=vs.90).aspx)
- [FileVersionInfo.GetVersionInfo](http://msdn.microsoft.com/en-us/library/system.diagnostics.fileversioninfo.getversioninfo(v=vs.110).aspx)