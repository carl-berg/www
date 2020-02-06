---
title: "Kicking the tires of Micronetes"
description: "A mini kubernetes inspired orchestrator for running microservices"
date: 2020-02-06
tags: ["Micronetes", "m8s", "Kubernetes", "Dotnet", "Microservices"]
---

## Why Micronetes (m8s)?

![](/img/micronetes-logo.svg)

My reason for trying micronetes is trying to resolve some of the friction caused by testing out features that span multiple services. You might start something off in the UI which might cause actions to ripple through several services and it can get tedious to startup everything you need to test the whole flow. Docker might be a bit cumbersome, especially when you have dotnet framework services you want to run. This is where Micronetes can potentially shine.

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

A nice feature is [Service Descriptions](https://github.com/davidfowl/Micronetes#service-descriptions), which means that configurations will be available (through environment variables and as `IConfiguration` keys) to all services/applications in the "cluster". So for instance, a port for a web-api can be exposed to another service.

There's also support for running docker containers from m8s, but I haven't yet tried that.

## Time to run your m8s "cluster"

This is the easiest part. Just run `m8s run my-services.yaml`, or whatever you named your yaml configuration file.
When you cluster starts a dashboard url show up in the logs. Here you can monitor your services and also view their logs.

![](/img/micronetes-dashboard.png)

## Customization

There are a few customizations you can do, like specifying how many instances of a service you want to run, which port you want the dashboard to run on for example. Check out the [documentation on github](https://github.com/davidfowl/Micronetes#micronetes-cli).

## Setting up credentials for azure devops artifacts

A problem i ran into right aways was that some projects referenced nuget packages from an Azure Devops artifact feed, which require credentials. Therefore i could not just simply run `dotnet restore` for those projects. After some googling I found _Azure artifacts credential provider_ (link below) that will prompt a link and a code for verification the first time. After that credentials will be cached and no prompt required. Not sure for how long, but im sure i will find out :smile:.

## Resources

-   [Micronetes](https://github.com/davidfowl/Micronetes)
-   [Azure artifacts credential provider](https://github.com/microsoft/artifacts-credprovider)
-   [Image credits: _all images have been borrowed from the github repo_](https://github.com/davidfowl/Micronetes)
