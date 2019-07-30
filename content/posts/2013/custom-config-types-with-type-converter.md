---
title: "Using custom types in config sections by specifying a TypeConverter"
description: "Using typeconverter to map config parameters to types"
date: 2013-12-20
tags: ["C#"]
---

I just found something neat that i decided to to a quick writeup about.

I have gone through a few phases of handling configuration files for applications. At first it seemed quite complicated so i just dumped everything in appSettings as key value pairs and then parsed the strings into whatever i needed. Lateley i have started using ConfigurationSections and ConfigurationElements more to build a structured configuration. That is alot nicer to look at when trying to find parameters, and you can also group properties togeather that belong to the same context.

Today i found something useful: How to use custom property types as configuration types. For example I wanted to have a configuration property for Encoding, but I would also like to use a typed encoding instead of just a string, so here's how to do that. At first you delare the property as usual, but you add a `TypeConverterAttribute` where you specify which TypeConverter to use.

```cs
public class FooConfiguration: ConfigurationElement
{
    [ConfigurationProperty("encoding", IsRequired = true), TypeConverter(typeof(EncodingTypeConverter))]
    public Encoding Encoding
    {
        get { return (Encoding)this["encoding"]; }
    }    
}
```

Then you go about implementing the type converter you need.
```cs
public class EncodingTypeConverter : TypeConverter
{
    public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType)
    {
        return sourceType == typeof(string);
    }

    public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value)
    {
        var val = value as string;
        return string.IsNullOrEmpty(val) ? Encoding.UTF8 : Encoding.GetEncoding(val);
    }

    public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType)
    {
        return destinationType == typeof(Encoding);
    }

    public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType)
    {
        var val = value as Encoding;
        return val != null ? val.ToString() : null;
    }
}
```

...and you're done!

This works for simple types where you can convert from and to a string, but I have not managed to make it work for complex objects with multiple properties. If anyone has any knowledge on how to do that, please let me know :-)

## Resources
[Type converter example](http://msdn.microsoft.com/en-us/library/87926x56.aspx)