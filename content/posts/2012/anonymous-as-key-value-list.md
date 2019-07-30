---
title: "Anonymous objects as key-value list"
description: "Using anonymous objects as key-value pair lists"
date: 2012-05-22
tags: ["CSharp"]
---

![Anonymous object](/img/man-with-paper-bag-on-head.jpg)

Lately, I have been using anonymous object quite alot as a way to pass in a number of key-value pairs into methods. This (I believe) is a quite nice way to expose a way to send an unknown number of key-value pairs into a method.

I first encountered this in ASP.NET MVC where it can be used for passing in route data or html attributes to generate an ActionLink. I tend to use it mostly when I need to work with unknown key-value pairs, such as html attributes or sql parameters.

Here's an example of how to use an anonymous object as a way of passing parameters into a sql call (using SqlCe in this example). 
```cs
protected virtual IEnumerable<SqlCeParameter> BuildParameters(object parameters)
{
    foreach (PropertyDescriptor propertyDecriptor in TypeDescriptor.GetProperties(parameters))
    {
        var name = propertyDecriptor.Name.StartsWith("@")
            ? propertyDecriptor.Name
            : "@" + propertyDecriptor.Name;
        var value = propertyDecriptor.GetValue(parameters);
        yield return new SqlCeParameter(name, value ?? DBNull.Value);
    }
}
```
And here is another example of a method call using an anonymous object.
```cs
public void Execute()
{
    Execute(@"INSERT INTO [User](FirstName, LastName, Email) 
        VALUES(@FirstName, @LastName, @Email)", 
        new { _user.FirstName, _user.LastName, _user.Email });
}
```

For complete code for this post, check out the github repository below.

## Resources
- [Demo Example](https://github.com/carl-berg/demo)