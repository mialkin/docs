# .NET CLI, `dotnet new`

The [↑ .NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/) is a cross-platform toolchain for developing, building, running, and publishing .NET applications.

## Table of contents

- [.NET CLI, `dotnet new`](#net-cli-dotnet-new)
  - [Table of contents](#table-of-contents)
  - [`build`](#build)
  - [`new`](#new)
    - [Create new solution](#create-new-solution)
    - [Item template](#item-template)

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
mkdir sample && cd sample
git init
dotnet new solution --name Sample
dotnet new gitignore
mkdir src
dotnet new webapi --name Sample.Api --output=src
dotnet sln add src/Sample.Api.csproj
dotnet run --project=src/Sample.Api.csproj
```

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
    "classifications": [ "Common", "Code" ],
    "identity": "ExampleTemplate.StringExtensions",
    "name": "Example templates: string extensions",
    "shortName": "stringext",
    "tags": {
      "language": "C#",
      "type": "item"
    },
    "symbols": {
      "ClassName":{
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
mkdir test && cd test
dotnet new stringext
```
