# Automated Static V8 Builds using Azure DevOps

This package contains pre-built V8 static libraaries and headers required to embed the V8 JavaScript engine into a C++ project.

> Note: This repository contains static V8 builds targeting VS2019 and a different set of GN_Options used for Espresso. See [this repository](https://github.com/pmed/v8-nuget) for dynamic libaries and additional platform toolsets.

| Package                     | Version
|-----------------------------|----------------------------------------------------------------------------------------------------------------------|
|V8 x86 for Visual Studio 2019|[![NuGet](https://img.shields.io/nuget/v/BaristaLabs.Espresso.v8-static.win-x86.svg)](https://www.nuget.org/packages/BaristaLabs.Espresso.v8-static.win-x86/)|
|V8 x64 for Visual Studio 2019|[![NuGet](https://img.shields.io/nuget/v/BaristaLabs.Espresso.v8-static.win-x64.svg)](https://www.nuget.org/packages/BaristaLabs.Espresso.v8-static.win-x64/)|

## Usage

To use V8 in a project install the package `BaristaLabs.Espresso.v8-static.win-$Platform.$Version`
from a console with `nuget install` command or from inside of Visual Studio
(see menu option *Tools -> NuGet Package Manager -> Manage NuGet Packages for Solution...*)
where

  * `$Platform` is a target platform type, currently `x86` or `x64`.

  * `$Version` is the actual V8 version, one of https://chromium.googlesource.com/v8/v8.git/+refs

After successful packages installation add `#include <v8.h>` in a C++  project
and build it. All necessary files (*.lib, *.dll, *.pdb) would be referenced
in the project automatically with MsBuild property sheets.

## Publishing

Visit https://omahaproxy.appspot.com/ for a list of the V8 versions that correspond to a Chrome build. 

Execute ```python build-win.py $Version```

Publish ```nuget push *.nupkg $NugetAPIKey -Source https://nuget.org/```

## How to build

This section is mostly for the package maintainers who wants to update V8.

Tools required to build V8 NuGet package on Windows:

  * Visual C++ toolset (version >=2013)
  * Python 2.X
  * Git
  * NuGet (https://dist.nuget.org/index.html)

To build V8 and make NuGet packages:

  1. Run `build-win.py` with optional command-line arguments.
  2. Publish `nuget/*.nupkg` files after successful build.
  
Build script `build-win.py` supports command-line arguments to specify package build options:

  1. V8 version branch/tag name (or `V8_VERSION` environment variable), default is `lkgr` branch
  2. Target platform (or `PLATFORM` evnironment variable), default is [`x86`, `x64`]
  3. Configuration (or `CONFIGURATION` environment variable), default is [`Debug`, `Release`]
