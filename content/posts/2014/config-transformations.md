---
title: "Config transforms"
description: "Making config transformations terse."
date: 2014-03-11
---

Today i have merged a branch with the same exact change in 5 config transforms. This could have been avoided if the proper transform approach had been used. Here's an example of how you can cleanup a sloppy config transform:

Before cleanup:
```xml
<?xml version="1.0" encoding="utf-8"?>
 <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
    <log4net xdt:Transform="Replace">
        <appender name="Console" type="log4net.Appender.ConsoleAppender">
            <layout type="log4net.Layout.PatternLayout">
                <param name="ConversionPattern" value="%d %-5p %c{1}:%L - %m%n" />
            </layout>
        </appender>
        <appender name="LogFileAppender" type="log4net.Appender.RollingFileAppender">
            <param name="File" value="C:\some\other\path\Default.log" />
            <param name="AppendToFile" value="true" />
            <rollingStyle value="Size" />
            <maxSizeRollBackups value="10" />
            <maximumFileSize value="10MB" />
            <staticLogFileName value="true" />
            <layout type="log4net.Layout.PatternLayout">
                <conversionPattern value="%date - %level - %logger - %message%newline" />
            </layout>
        </appender>
        <appender name="AppLog" type="log4net.Appender.RollingFileAppender">
            <param name="File" value="C:\some\other\path\AppLog.log" />
            <param name="AppendToFile" value="true" />
            <rollingStyle value="Size" />
            <maxSizeRollBackups value="10" />
            <maximumFileSize value="10MB" />
            <staticLogFileName value="true" />
            <layout type="log4net.Layout.PatternLayout">
                <conversionPattern value="%date - %level - %logger - %message%newline" />
            </layout>
        </appender>
        <root>
            <level value="ALL" />
            <appender-ref ref="LogFileAppender" />
            <appender-ref ref="Console" />
        </root>
        <logger name="Application-Log" additivity="false">
            <level value="ALL" />
            <appender-ref ref="AppLog" />
            <appender-ref ref="Console" />
        </logger>
    </log4net>
</configuration>
 ```
After cleanup:
```xml
<?xml version="1.0" encoding="utf-8"?>
 <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
    <log4net>
        <appender name="LogFileAppender" xdt:Locator="Match(name)">
            <param name="File" value="C:\some\other\path\Default.log" xdt:Transform="SetAttributes(value)" xdt:Locator="Match(name)" />
        </appender>
        <appender name="AppLog" xdt:Locator="Match(name)">
            <param name="File" value="C:\some\other\path\AppLog.log" xdt:Transform="SetAttributes(value)" xdt:Locator="Match(name)" />
        </appender>
    </log4net>
</configuration>
 ```
So to sum it up, don't use Replace unless you have to.

## Resources
- [MSDN](http://msdn.microsoft.com/en-us/library/dd465326(v=vs.110).aspx)