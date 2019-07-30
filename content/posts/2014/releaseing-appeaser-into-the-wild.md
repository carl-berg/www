---
title: "Get ready to be Appeased"
description: "New open sourced mediator library"
date: 2014-04-27
tags: ["Appeaser", "Mediator"]
---

![Appeaser](https://raw.githubusercontent.com/carl-berg/appeaser/master/res/icon_256.png)

Last week I made my first open source release, a mediator implementation in C# called [Appeaser](https://github.com/carl-berg/appeaser). I got insipred by [a series of blog posts](http://lostechies.com/jimmybogard/2013/12/19/put-your-controllers-on-a-diet-posts-and-commands/) by Jimmy Bogard about putting your controllers on a diet, which is pretty much in tune with previous attempts I have made of removing repositories in favour of a command/query based pattern. I then ended up by putting my command and query execution logic in the controllers which never felt quite clean and required me to copy paste abstract base controller logic from project to project.

Enter the mediator pattern. I could now end up with code like this in my controllers (validation handling taken care of elsewhere):

```cs
public class PersonController : Controller
{
    //...

    public void ActionResult Index(PagingParameters paging, SortingParameters sorting)
    {
        var viewModel = _mediator.Request(new UserListQuery(paging, sorting));
        return View(viewModel);
    }

    public void ActionResult Edit(Guid id)
    {
        var viewModel = _mediator.Request(new UserFormQuery(id));
        return View(viewModel);
    }

    //...
}
```

Read more about using the appeaser mediator on [it's github page](https://github.com/carl-berg/appeaser). There's also [a version out on nuget](https://www.nuget.org/packages/Appeaser/) as well as [a pre-release version with async support](https://www.nuget.org/packages/Appeaser/1.0.2-alpha).

## Resources
- [Github project](https://github.com/carl-berg/appeaser)
- [Nuget package](https://www.nuget.org/packages/appeaser)