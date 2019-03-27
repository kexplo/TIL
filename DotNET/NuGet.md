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

Add Multiple Sources to `NuGet.Config` file

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="my private nuget source" value="http:/...." />
    <add key="offical nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

Copy `NuGet.Config` to Config file locations.

Config file locations:

- `$HOME/.config/NuGet/NuGet.Config`(or `$HOME/.nuget/NuGet/NuGet.Config`).
- Solution Root Directory
- Project Root Directory

SEE [NuGet Config file locations](https://docs.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior#config-file-locations-and-uses)

**IMPORTANT**: the paths and the `NuGet.Config` file name are CASE SENSITIVE.

And, Run `dotnet restore` or `dotnet restore --configfile <NuGet.Config location>`

If above instructions doesn't work, it is a bug. there is [a issue](https://github.com/NuGet/Home/issues/6140).

You can temporarily fix the bug by following the instruction below:

```bash
$ dotnet restore --source "http://private-nuget-repo-source" || true
$ dotnet restore --source "https://api.nuget.org/v3/index.json" || true
$ dotnet restore --source "https://api.nuget.org/v3/index.json;http://private-nuget-repo-source"
```
