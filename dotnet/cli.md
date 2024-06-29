# .NET CLI, `dotnet`

The [↑ .NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/) is a cross-platform toolchain for developing, building, running, and publishing .NET applications.

## Table of contents

- [.NET CLI, `dotnet`](#net-cli-dotnet)
  - [Table of contents](#table-of-contents)
  - [`build`](#build)
  - [`new`](#new)
    - [Create new solution](#create-new-solution)

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
