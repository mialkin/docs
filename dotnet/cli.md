# .NET CLI, `dotnet new`

The [↑ .NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/) is a cross-platform toolchain for developing, building, running, and publishing .NET applications.

## Table of contents

- [.NET CLI, `dotnet new`](#net-cli-dotnet-new)
  - [Table of contents](#table-of-contents)
  - [`build`](#build)
  - [`new`](#new)
    - [Create new solution](#create-new-solution)
  - [Templates](#templates)
    - [Item template](#item-template)
    - [Project template](#project-template)
    - [Template package](#template-package)

## `build`

The [↑ `dotnet build`](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-build) command builds a project and all of its dependencies.

```bash
dotnet build --configuration Release /warnaserror
```

`/warnaserror` — treat all warnings as errors.

## `new`

The [↑ `dotnet new`](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new) command creates a new project, configuration file, or solution based on the specified template.

### Create new solution

```bash
mkdir sample && cd $_
git init
dotnet new solution --name Sample
dotnet new gitignore
mkdir src
dotnet new webapi --name Sample.Api --output=src
dotnet sln add src/Sample.Api.csproj
dotnet run --project=src/Sample.Api.csproj
```

## Templates

### Item template

[↑ Tutorial: Create an item template](https://learn.microsoft.com/en-us/dotnet/core/tutorials/cli-templates-create-item-template).

```bash
mkdir working/content/extensions/.template.config
cd working/content/extensions
touch StringExtensions.cs
touch .template.config/template.json
```

`StringExtensions.cs` file:

```csharp
namespace System;

public static class StringExtensions
{
    public static string Reverse(this string value)
    {
        char[] tempArray = value.ToCharArray();
        Array.Reverse(tempArray);
        return new string(tempArray);
    }
}
```

`template.json` file:

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": ["Common", "Code"],
  "identity": "ExampleTemplate.StringExtensions",
  "name": "Example templates: string extensions",
  "shortName": "stringext",
  "tags": {
    "language": "C#",
    "type": "item"
  },
  "symbols": {
    "ClassName": {
      "type": "parameter",
      "description": "The name of the code file and class.",
      "datatype": "text",
      "replaces": "StringExtensions",
      "fileRename": "StringExtensions",
      "defaultValue": "StringExtensions"
    }
  }
}
```

Install template:

```bash
dotnet new install ./
```

Uninstall template:

```bash
dotnet new uninstall ./
```

Test template:

```bash
mkdir test && cd $_
dotnet new stringext
```

### Project template

```bash
cd working/content
mkdir consoleasync && cd $_
dotnet new console
```

`Program.cs`:

```csharp
await Console.Out.WriteAsync("Hello World with C#");
```

```bash
mkdir .template.config && cd $_
touch template.json
```

`template.json`:

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": ["Common", "Console"],
  "identity": "ExampleTemplate.AsyncProject",
  "name": "Example templates: async project",
  "shortName": "consoleasync",
  "sourceName": "consoleasync",
  "tags": {
    "language": "C#",
    "type": "project"
  }
}
```

```bash
dotnet new install ./
```

```bash
dotnet new consoleasync --name MyProject
```

```bash
dotnet new uninstall ./
```

### Template package

A [↑ template package](https://learn.microsoft.com/en-us/dotnet/core/tutorials/cli-templates-create-template-package) is one or more templates packed into a NuGet package.

When you install or uninstall a template package, all templates contained in the package are added or removed, respectively.

Install the Microsoft.TemplateEngine.Authoring.Templates template from the NuGet package feed:

```bash
dotnet new install Microsoft.TemplateEngine.Authoring.Templates
```

Template packages are represented by a NuGet package, .nupkg, file. And, like any NuGet package, you can upload the template package to a NuGet feed.

```bash
cd working
dotnet new templatepack --name "AdatumCorporation.Utility.Templates"
```

The `content` content folder contains a `SampleTemplate` folder. Delete this folder, as it was added to the authoring template for demonstration purposes.

```bash
dotnet pack
```

```bash
dotnet new install bin/Release/AdatumCorporation.Utility.Templates.1.0.0.nupkg
```

If you uploaded the NuGet package to a NuGet feed, you can use the `dotnet new install <PACKAGE_ID>` command where `<PACKAGE_ID>` is the same as the `<PackageId>` setting from the `.csproj` file.

To uninstall the template package:

```bash
dotnet new uninstall AdatumCorporation.Utility.Templates
```
