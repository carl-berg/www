---
title: "Version 2.3 of Appeaser"
description: "Maintenance release adding IOptions<MediatorSettings> support"
date: 2019-08-01
tags: ["Appeaser", "Mediator"]
---
I have recently done a little maintenance update in Appeaser where i've added `netstandard 2.0` compatibility for [Appeaser](https://www.nuget.org/packages/Appeaser/2.3.0) and added support for `IOptions<MediatorSettings>` in [Appeaser.Microsoft.DependencyInjection](https://www.nuget.org/packages/Appeaser.Microsoft.DependencyInjection/1.1.0) making use of the ["options pattern"](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-2.2).

The latter of these updates enables you to configure mediator settings like this:
```cs
var serviceProvider = new ServiceCollection()
    .Configure<MediatorSettings>(o => o.AddRequestInterceptor<MyInterceptor>())
    .Configure<MediatorSettings>(o => o.AddRequestInterceptor<MyOtherInterceptor>())
    .AddAppeaser()
    .BuildServiceProvider();
```
...the benefit which might not be completely obvious compared to:
```cs
var serviceProvider = new ServiceCollection()
    .AddAppeaser(options => options
        .AddRequestInterceptor<MyInterceptor>()
        .AddRequestInterceptor<MyOtherInterceptor>())
    .BuildServiceProvider();
```
...however this enables you to make something like this:
```cs
public static IServiceCollection RegisterValidation(this IServiceCollection services)
{
    services.Configure<MvcOptions>(o => o.Filters.Add<HandleValidationExceptionActionFilter>());
    services.Configure<MediatorSettings>(o => o.AddRequestInterceptor<ValidationInterceptor>());
    return services;
}
```
... where an extension (that could be shared in a nuget package for instance) configures one or more options but doesn't need direct access to the `AddAppeaser(..)` or `AddMvc(..)` methods, also the order these configurations are invoked doesn't matter as creation of these options happen at a later stage.

## Resources
- [Release notes](https://github.com/carl-berg/appeaser/releases/tag/2.3.0)