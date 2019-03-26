# Nuget

## Install local NuGet Package File

Add a `Nuget.Config` file to Project Directory

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="local" value="..\NugetPackages" />
  </packageSources>
</configuration>
```

Copy the `*.nupkg` files to `..\NugetPackages` Directory.

Run Command:

```cmd
> dotnet add package <PackgeName>
```

Alternatively in .NET Core 2.0 tools / NuGet 4.3.0, you could also add the source directly to the csproj file that is supposed to consume the NuGet:

```xml
<PropertyGroup>
  <RestoreSources>$(RestoreSources);../foo/bin/Debug;https://api.nuget.org/v3/index.json</RestoreSources>
</PropertyGroup>
```

reference: https://stackoverflow.com/a/44463578

## Install packages from Multiple sources

Add Multiple Sources to `Nuget.Config` file

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="my private nuget source" value="http:/...." />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

Copy `Nuget.Config` to `$HOME/.config/Nuget/Nuget.Config`(or `$HOME/.nuget/Nuget/Nuget.Config`).
SEE [NuGet Config file locations](https://docs.microsoft.com/ko-kr/nuget/consume-packages/configuring-nuget-behavior#config-file-locations-and-uses)

And, Run `dotnet restore`

**NOTE**: Do not use `dotnet restore --configfile <Nuget.Config location>` or `dotnet restore --source http://.. --source http://..`. These options can't install packages from multiple sources. I think it is a bug (from .NET Core 2.2.1.05 Ubuntu 16.04)

see also, https://github.com/NuGet/Home/issues/6140
