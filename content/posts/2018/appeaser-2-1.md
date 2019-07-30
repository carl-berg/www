---
title: "Version 2.1 of Appeaser"
description: " Some new stuff in released in Appeaser, your favourite mediator"
date: 2018-08-24
tags: ["Appeaser", "Mediator"]
---

I've finally had a little time over to implement some stuff in Appeaser that i've had in my mind for a while. The new version shouldn't introduce any breaking changes but adds some simplifications like the `IRequest` interface that is implemented by both queries and commands. This also enables use of a simplified mediator (`ISimpleMediator`) that some might prefer that just contains a `Request` method and can be used like this:
```cs
var something = await mediator.Request(new Feature.Request());
```
with something like this as the implementation:
```cs
public class Feture
{
    public class Request : IAsyncRequest<Response> { }

    public class Handler : IAsyncRequestHandler<Request, Response>
    {
        public Response Handle(Request request) => new Response();
    }

    public class Response { }
}
```
Another benefit of the new interface is that it can simplify the Handler scanning (as both query and command handlers inherit from request handler) in the IOC configuration like so:
```cs
// For Lamar
public class MyLamarRegistry : ServiceRegistry
{
    public MyLamarRegistry()
    {
        For<IMediator>().Use<Mediator>();
        For<IMediatorHandlerFactory>().Use<MediatorHandlerFactory>();
        Scan(s =>
        {
            s.TheCallingAssembly();
            s.ConnectImplementationsToTypesClosing(typeof(IRequestHandler<,>));
            s.ConnectImplementationsToTypesClosing(typeof(IAsyncRequestHandler<,>));
        });			
    }
}

// For StructureMap
public class MyStructuremapRegistry : Registry
{
    public MyStructuremapRegistry()
    {
        For<IMediator>().Use<Mediator>();
        For<IMediatorSettings>().Use<MediatorSettings>();
        For<IMediatorHandlerFactory>().Use<MediatorHandlerFactory>();
        Scan(s =>
        {
            s.TheCallingAssembly();
            s.ConnectImplementationsToTypesClosing(typeof(IRequestHandler<,>));
            s.ConnectImplementationsToTypesClosing(typeof(IAsyncRequestHandler<,>));
        });			
    }
}
```
Another feature that wasn't really announced, but released in version 2.0 was the introduction of `MediatorSettings`, which as of now only implement one setting and that's wether exceptions thrown in handlers should be wrapped by mediator exceptions or not. The default value (for backwards compatability) is to wrap exceptions.

## Resources
- [Release notes](https://github.com/carl-berg/appeaser/releases/tag/2.1.0)