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