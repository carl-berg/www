---
title: "Self Hosted Owin WebApi with Structuremap DependencyResolver gotcha"
description: "Fixing StackOverflowException when disposing a self-hosted Owin WebApi -process"
date: 2015-06-17
tags: ["Owin", "Test"]
---

After a lot of debugging I have just solved an issue with an integration test running against an owin selfhosted Web Api. 
The problem i was experiencing was that the code throwing a StackOverflowException when the selfhosted process was disposed:
```cs
[Binding]
public class GlobalSetupTearDown
{
    private static IDisposable _webApp;
        
    [BeforeTestRun]
    public static void Setup()
    {
        _webApp = WebApp.Start<StartupSpecs>(Settings.TestPath);
    }

    [AfterTestRun]
    public static void TearDown()
    {
        _webApp.Dispose(); // <-- Throws a StackOverflowException
    }
}
```
The issue I was experiencing was related to using [WebApiContrib.IoC.StructureMap](https://github.com/WebApiContrib/WebApiContrib.IoC.StructureMap). The actual issue was due to that the Structuremap Container contained an instance to the `StructureMapResolver` for `IHttpControllerActivator`. However when `StructureMapResolver` is being disposed, it will attempt to dispose the container which causes some kind of recursive mess resulting in a `StackOverflowException`. My solution is not particularly pretty, but seems to solve the problem:
```cs
[Binding]
public class GlobalSetupTearDown
{
    // ..snip
    [AfterTestRun]
    public static void TearDown()
    {
        [AfterTestRun]
        public static void TearDown()
        {
            // Manually eject the IHttpControllerActivator (DependencyResolver) to avoid a StackOverflowException
            ObjectFactory.EjectAllInstancesOf<IHttpControllerActivator>();
            _webApp.Dispose();
        }
    }
}
```