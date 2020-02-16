[Cascadia PL](https://github.com/microsoft/cascadia-code/releases)

### Terminal settings
```json
{
    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}" /* points to the powershell core profile */,
    "requestedTheme": "dark",
    "profiles":
    {
        "defaults":
        {
            "fontFace": "Cascadia Code PL",
            "fontSize": 10
        },
    }
}
```

### 

```powershell
# Install posh-git and oh-my-posh
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
Install-Module -Name PSReadLine -AllowPrerelease -Scope CurrentUser -Force -SkipPublisherCheck # Since i use PS Core
```

Configure `~\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`
```powershell
# Setup my profile
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Paradox

# TODO 
```