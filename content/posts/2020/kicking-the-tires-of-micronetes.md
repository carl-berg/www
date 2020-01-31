---
title: "Kicking the tires of Micronetes"
description: "A mini kubernetes inspired orchestrator for running microservices"
date: 2020-01-31
tags: ["Micronetes", "Kubernetes", "Dotnet", "Microservices"]
draft: true
---

## Why Micronetes (m8s)?

Recently in working with microservices I have found that there's a bit of overhead in testing out features that span several services. You might start something off in the UI which might cause actions to ripple through several services and it can get tedious to startup everything you need to test the whole flow. Docker might be a bit heavy, especially when you're also running dotnet full-framework services. This is where Micronetes can potentially shine.

`Disclaimer, Micronetes is currently in alpha so things might change...`

I've been trying out m8s on a client project that's been around for a few years which means we have a plethora of different stuff to run but they all fall in mainly these categories:

1. asp.net core web projects
2. full framework web projects
3. dotnet core windows services
4. full framework windows services (using newer `csproj`-format)
5. node frontend's

...and their setup in m8s all differ slightly. You can run m8s by pointing it to a `.sln` file but if you have different type of services to run, i would recommend that you create your own definition in `yaml`.

### Setup for some different types of project

```yaml
# Asp.net core project using new csproj format
- name: my.api
  project: MyApi.csproj

# Full framework service using new csproj format
- name: my.service
  project: MyService\MyService.csproj

# Full framework web project using old csproj format
- name: my.web.ui
  executable: C:\Program Files (x86)\IIS Express\iisexpress.exe # This path could differ. Check your local machine
  args: /config:C:\Develop\MyProj\.vs\MyProj\config\applicationhost.config /site:My.Web.UI
  bindings:
      - port: 12345

# Full framework service using old csproj format
- name: my.older.service
  executable: MyOlderService\bin\Debug\MyOlderService.exe

# Node web api
- name: my.node.api
  workingDirectory: ..\MyNodeApi
  executable: C:\Program Files\nodejs\node.exe # This path could differ. Check your local machine
  args: server.js
```

Some things to note about the configuration above. m8s is currently optimized for dotnet core and will automatically build your project and can pick up ports for `launchSettings.json` automatically. Other types of project you can run there's not build support out of the box.

One gotcha i ran into here was that we are using a private nuget feed in Azure Devops which prevents `dotnet restore` from being run successfully (credentials are needed). Luckily i found [Azure artifacts credentials provider](#setting-up-credentials-for-azure-devops-artifacts) that will help you set up nuget credentials for Azure Devops Artifacts.

## Time to run your m8s "cluster"

This is the easiest part. Just run `m8s run my-services.yaml`, or whatever you named your yaml configuration file.
When you cluster starts you will get a dashboard url show up in the logs. Here you can monitor your services and also view their logs.

TODO: Replace image
![](https://user-images.githubusercontent.com/95136/72794899-95868680-3bf1-11ea-8625-fcb31218570b.png)

## Customization

TODO: Ports, Instances etc

## Setting up credentials for azure devops artifacts

TODO

## Resources

-   [Micronetes](https://github.com/davidfowl/Micronetes)
-   [Azure artifacts credential provider](https://github.com/microsoft/artifacts-credprovider)
