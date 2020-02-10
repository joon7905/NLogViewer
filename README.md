[1]: https://github.com/yarseyah/sentinel
[2]: https://github.com/dojo90/NLogViewer/blob/master/src/NLogViewer/Targets/CacheTarget.cs
[3]: https://github.com/yarseyah/sentinel#nlogs-nlogviewer-target-configuration

[p1]: doc/images/control.png "NLogViewer"
[p2]: doc/images/live.gif "NLogViewer"
[p3]: doc/images/colors.png "NLogViewer"

[nuget]: https://nuget.org/packages/Sentinel.NlogViewer/

## Nuget

[![NuGet](https://img.shields.io/nuget/v/sentinel.nlogviewer.svg "nuget")](https://www.nuget.org/packages/Sentinel.NLogViewer)
[![NuGetDownloads](https://img.shields.io/nuget/dt/sentinel.nlogviewer.svg "nuget downloads")](https://www.nuget.org/packages/Sentinel.NLogViewer)

A NuGet-package is available [here][nuget].

NlogViewer
==========

NlogViewer is a ui control library to visualize NLog logs in your personal application. It is mainly based on [Sentinel][1] and its controls.

supported Frameworks: `.NETCoreApp v3.0`, `.NETCoreApp v3.1`, `.NET Framework 4.6.1`

![NLogViewer][p2]

## Quick Start

Add a namespace to your `Window`

```xaml
xmlns:dj="clr-namespace:DJ;assembly=NLogViewer"
```

use the control
```xaml
<dj:NLogViewer/>
```

`NlogViewer` is subscribing to [CacheTarget][2]. By default, the `NlogViewer` is automatically creating a [CacheTarget][2] with `loggingPattern  "*"` and `LogLevel "Trace"`.

If you want to customize the `loggingPattern` and `LogLevel`, add the following to your `Nlog.config`.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog 
  xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.nlog-project.org/schemas/NLog.xsd NLog.xsd"
  autoReload="true">

  <extensions> 
    <add assembly="NLogViewer"/> 
  </extensions> 

  <targets async="true">
    <target
      xsi:type="CacheTarget"
      name="cache"/>
  </targets>

  <rules>
    <logger name="*" writeTo="cache" minlevel="Debug"/> 
  </rules>
</nlog>
```

## Customize

### Colors

Customize `foreground` or `background` of every `logLevel`

![NLogViewer][p3]

### Multi targeting

Use more than one instance of `NLogViewer` to match different `rules`.

Create 2 `targets` with their own `rules`.

```xml
<targets async="true">
  <target xsi:type="CacheTarget" name="target1"/>
  <target xsi:type="CacheTarget" name="target2"/>
</targets>

<rules>
  <logger name="*" writeTo="target1" maxlevel="Info"/> 
  <logger name="*" writeTo="target2" minlevel="Warn"/> 
</rules>
```

Set `TargetName` property to link them.

```xml
<dj:NLogViewer TargetName="target1"/>
<dj:NLogViewer TargetName="target2"/>
```

### Format output (ILogEventInfoResolver)

To format the output of a `LogEventInfo`, implement a new instance of `ILogEventInfoResolver` and bind it to the `Resolver` you want to customize:

```csharp
/// <summary>
/// Reformat the DateTime
/// </summary>
public class FooTimeStampResolver : ILogEventInfoResolver
{
    public string Resolve(LogEventInfo logEventInfo)
    {
        return logEventInfo.TimeStamp.ToUniversalTime().ToString();
    }
}
```

```csharp
NLogViewer1.TimeStampResolver = new FooTimeStampResolver();
```

## Why CacheTarget?

There is already a `NLogViewerTarget`, which is used for [Sentinel][1]. See [here][3]

```xml
<target 
    xsi:type="NLogViewer"
    name="sentinel"
    address="udp://127.0.0.1:9999"/>
```

## Contributors

Feel free to make a PullRequest or open an Issue to extend this library!