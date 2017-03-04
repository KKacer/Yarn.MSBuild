Yarn.MSBuild
============

An MSBuild task for running the Yarn package manager.

# Installation

```
PM> Install-Package Yarn.MSBuild
```

Manually install by editing .csproj and adding a `PackageReference` to `Yarn.MSBuild`.

# Usage

## Default usage

This package is designed for use with ASP.NET Core projects.

```xml 
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.0" />
    <PackageReference Include="Yarn.MSBuild" Version="0.21.3" />
  </ItemGroup>
</Project>
```

Project layout:
```
+ WebApplication.csproj
+ package.json
+ Startup.cs
- wwwroot
   + app.js
   + site.css
```

Running `dotnet build` or `msbuild.exe /t:Build` will automatically invoke `yarn install`.
Running `dotnet clean` or `msbuild.exe /t:Clean` will automatically invoke `yarn clean`.

### Additional options

```xml
<PropertyGroup>
  <!-- Prevent yarn from running on 'Build' and 'Clean'. -->
  <SuppressAutoYarn>true</SuppressAutoYarn>
  <!-- Change the directory in which yarn is invoked on build/clean. Defaults to MSBuildProjectDirectory. -->
  <YarnDir>wwwroot/</YarnDir>
</PropertyGroup>
```

## Using the task 

The `Yarn` task supports the following parameters
```
[Optional]
string Command             The arguments to pass to yarn.

[Optional]
string ExecutablePath      Where to find yarn (*nix) or yarn.cmd (Windows)

[Optional]
string WorkingDirectory    The directory in which to execute the yarn command
```

```xml
<Project>
  <Target Name="Build">
    <!-- defaults to "install" in the current directory using the bundled version of yarn. -->
    <Yarn /> 

    <Yarn Command="upgrade" />

    <Yarn Command="run test" WorkingDirectory="wwwroot/" />
    <Yarn Command="run cmd" ExecutablePath="/usr/local/bin/yarn" />
  </Target>
</Project>
```
