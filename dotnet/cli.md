# .NET CLI

The .NET command-line interface (CLI) is a cross-platform toolchain for developing, building, running, and publishing .NET applications.

[.NET CLI documentation ↑](https://docs.microsoft.com/en-us/dotnet/core/tools/)

| <div style="width:120px">Command</div> | Description                                                                                                           |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| dotnet build                           | Build a project and all of its dependencies                                                                           |
| dotnet clean                           | Clean the output of the previous build                                                                                |
| dotnet msbuild                         | Build a project and all of its dependencies; allows access to a fully functional MSBuild                              |
| dotnet new                             | Create a new project, configuration file, or solution based on the specified template                                 |
| dotnet new --list                      | List of all installed templates                                                                                       |
| dotnet pack                            | Build the project and create NuGet packages. The result of this command is a NuGet package (that is, a _.nupkg_ file) |
| dotnet publish                         | Publish the application and its dependencies to a folder for deployment to a hosting system                           |
| dotnet restore                         | Restore the dependencies and tools of a project <sup>1</sup>                                                          |
| dotnet run                             | Run source code without any explicit compile or launch commands <sup>2</sup>                                          |
| dotnet test                            | Execute unit tests in a given solution <sup>3</sup>                                                                   |

## Footnotes

<sup>1</sup> In most cases, you don't need to explicitly use the dotnet restore command, since a NuGet restore is run implicitly if necessary when you run the following commands:

```bash
dotnet new
dotnet build
dotnet build-server
dotnet run
dotnet test
dotnet publish
dotnet pack
```

<sup>2</sup> The command depends on the `dotnet build` command to build the code. Any requirements for the build, such as that the project must be restored first, apply to `dotnet run` as well.

The `dotnet run` command is used in the context of projects, not built assemblies. If you're trying to run a framework-dependent application DLL instead, you must use `dotnet` without a command:

```bash
dotnet myapp.dll
```

<sup>3</sup> The `dotnet test` command builds the solution and runs a test host application for each test project in the solution. The test host executes tests in the given project using a test framework, for example: MSTest, NUnit, or xUnit, and reports the success or failure of each test. If all tests are successful, the test runner returns `0` as an exit code; otherwise if any test fails, it returns `1`.
