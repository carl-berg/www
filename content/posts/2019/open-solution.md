---
title: "Open Visual Studio solutions below current folder"
description: "An update on previous powershell function"
date: 2019-12-20
tags: ["Terminal", "Powershell", "Visual Studio"]
---

A few years ago [in a previous post]({{< ref "/posts/2015/open-visual-studio-from-powershell" >}}) i displayed a little function that can open a solution below the current folder. The obvious problem is if you have more than one solution below you current folder, then only the first solution file found was opened. So here's a revamped version that will open a solution if only one is found, otherwise you will be prompted for which one you want to open.

```powershell
function sln {

    Param([Alias("f")][switch]$First)

    if ($First -eq $true) {
        $file = Get-ChildItem -recurse *.sln |
                Sort-Object -Property { $_.DirectoryName.Length -as [int] } |
                Sort-Object -Property { $_.Name } |
                Select-Object -First 1
        if ($file) {
            Start-Process -FilePath $file.FullName
        }
    }
    else {
        [array]$files = Get-ChildItem -Recurse *.sln |
            ForEach-Object { Write-Progress -Id 1 -Activity "Looking for solutions" -Status "Found $($_.Name)"; $_ } |
            Select-Object Name, FullName

        if ($files.Length -eq 1) {
            Start-Process -FilePath $files[0].FullName
        }
        elseif ($files.Length -gt 1) {
            Write-Host "Found more than one solution"
            $files |
                ForEach-Object { $index = 0 } { $_; $index++ } |
                Format-Table -Property @{ Label = "Number"; Expression = { $index }; }, Name
            $SelectedIndex = Read-Host -Prompt 'Press corresponding number to open: '
            Start-Process -FilePath $files[$SelectedIndex].FullName
        }
        else {
            Write-Host "Found no solution file here. Sorry"
        }
    }
}
```

As a nice touch :smirk:, it will also display it's progress (can be slow if there are lots of files and folders below).
