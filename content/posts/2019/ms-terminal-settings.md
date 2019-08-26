---
title: "Windows Terminal and Powershell configuration"
description: "A reminder on how i like to configure Windows Terminal"
date: 2019-08-26
tags: ["Terminal", "Powershell"]
---
A have been using the new Windows Terminal since it came out in Microsoft Store as a preview. I have also been tweaking a few things to get it working the way i like it.

![](/img/terminal.png)

## Before installing Windows Terminal
In order to get the terminal to pick up existing shells, first install Powershell Core and Windows Subsystem for Linux (WSL). If you do this before installing the terminal, old powershell will be replaced by the newer core version and WSL will also be picked up automatically.

## Configure Powershell Core
Add `%UserProfile%\Documents\PowerShell\Profile.ps1` if it doesn't already exist. After that i start by installing my favourite modules:

- Posh Git: `Install-Module -Name posh-git` 
- Oh My Posh: `Install-Module -Name oh-my-posh`

After that i tweak the powershell profile to look like this (so i auto load the modules above, and also custom functions):

```powershell
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme paradox

Import-Module ZLocation

# Custom functions
function e. {
    explorer .
}
```

## Install a nice font with ligatures
This is sort of needed in order to get _oh-my-posh_ to shine. I have tried some different ones that didn't look so good, but i settled for [Hasklig](https://github.com/i-tu/Hasklig), that i think looks spiffy with my setup. Download and install the font in windows.

## Configure Windows Terminal
Now that powershell is configured the way i like it, it's time to set up the terminal the way i like it.
I have set up the powershell profile like this (some properties removed for brevity)
```json
"profiles" : 
[
    {
        "acrylicOpacity" : 0.5,
        "background" : "#000000",
        "backgroundImage" : "C:/some-image-i-like.jpg",
        "backgroundImageOpacity" : 0.99989998340606689,
        "backgroundImageStretchMode" : "uniformToFill",
        "colorScheme" : "CB",
        "cursorColor" : "#FFFFFF",
        "cursorShape" : "bar",
        "fontFace" : "Hasklig",
        "fontSize" : 13,
        "snapOnInput" : true,
        "startingDirectory" : "C:/Develop/",
        "useAcrylic" : false
    },
]
```
Which references this color scheme:
```json
"schemes" : 
[
    {
        "background" : "#000000",
        "black" : "#0C0C0C",
        "blue" : "#0030AA",
        "brightBlack" : "#767676",
        "brightBlue" : "#3B78FF",
        "brightCyan" : "#61D6D6",
        "brightGreen" : "#16C60C",
        "brightPurple" : "#B4009E",
        "brightRed" : "#E74856",
        "brightWhite" : "#F2F2F2",
        "brightYellow" : "#F9F1A5",
        "cyan" : "#3A96DD",
        "foreground" : "#F2F2F2",
        "green" : "#40DD00",
        "name" : "CB",
        "purple" : "#881798",
        "red" : "#C50F1F",
        "white" : "#CCCCCC",
        "yellow" : "#C19C00"
    }
]
```


## Resources
- [Windows Terminal (Preview)](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot%3Aoverviewtab)
- [Powershell core](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6)
- [Posh git](https://github.com/dahlbyk/posh-git)
- [Oh my posh](https://github.com/JanDeDobbeleer/oh-my-posh)