# .NET tools, `dotnet-ef`, `dotnet-format`

A **.NET tool** is a special NuGet package that contains a console application.

| Command                                  | Description                   |
| ---------------------------------------- | ----------------------------- |
| dotnet tool list --global                | List globally installed tools |
| dotnet tool install --global TOOL_NAME   | Install tool globally         |
| dotnet tool update --global TOOL_NAME    | Update tool                   |
| dotnet tool uninstall --global TOOL_NAME | Uninstall tool                |

## Table of contents

- [.NET tools, `dotnet-ef`, `dotnet-format`](#net-tools-dotnet-ef-dotnet-format)
  - [Table of contents](#table-of-contents)
  - [`dotnet-ef`](#dotnet-ef)
    - [Installation](#installation)
    - [Base commands](#base-commands)
    - [Migrations](#migrations)
      - [Applying migrations](#applying-migrations)
        - [Migration bundles](#migration-bundles)
  - [`dotnet format`](#dotnet-format)
    - [Commands](#commands)
    - [`.editorconfig` examples](#editorconfig-examples)
    - [Code analyzers](#code-analyzers)
      - [Microsoft.VisualStudio.Threading.Analyzers](#microsoftvisualstudiothreadinganalyzers)
        - [Rule prefixes](#rule-prefixes)
    - [Other code formatters](#other-code-formatters)

## `dotnet-ef`

[↑ `dotnet-ef`](https://learn.microsoft.com/en-us/ef/core/cli/dotnet) is a cross-platform command line tooling that is used to manage database migrations and to scaffold database context.

### Installation

```bash
dotnet tool install dotnet-ef --global
```

Before you can use the tools on a specific project, you'll need to add the `Microsoft.EntityFrameworkCore.Design` package to it.

### Base commands

| Command                   | Meaning                                                                |
| ------------------------- | ---------------------------------------------------------------------- |
| dotnet ef database drop   | Deletes the database                                                   |
| dotnet ef database update | Updates the database to the last migration or to a specified migration |
| dotnet ef dbcontext info  | Gets information about a DbContext type                                |
| dotnet ef dbcontext list  | Lists available DbContext types                                        |

### Migrations

| Command                                 | Meaning                               |
| --------------------------------------- | ------------------------------------- |
| dotnet ef migrations add MIGRATION_NAME | Create new migration                  |
| dotnet ef migrations list               | Lists available migrations            |
| dotnet ef migrations remove             | Remove the last migration             |
| dotnet ef migrations script             | Generate a SQL script from migrations |

> Set up environment variable containing connection string before running commands below:

```bash
export PostgresSettings__ConnectionString="UserID=dictionary_mialkin_ru;Password=dictionary_mialkin_ru;Host=localhost;Port=5444;Database=dictionary_mialkin_ru;Pooling=true;Integrated Security=true"
```

List migrations:

```bash
dotnet ef migrations list \
--project src/Mialkin.Dictionary.Migrations \
--startup-project src/Mialkin.Dictionary
```

Create migration:

```bash
dotnet ef migrations add NewMigration \
--project src/Mialkin.Dictionary.Migrations \
--startup-project src/Mialkin.Dictionary
```

Create bundle:

```bash
dotnet ef migrations bundle \
--startup-project src/Mialkin.Dictionary \
--project src/Mialkin.Dictionary.Migrations \
--verbose \
--self-contained \
--output migration_bundle \
--force
```

Apply migration:

```bash
./migration_bundle
```

[↑ Using a Separate Migrations Project](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/projects?tabs=dotnet-core-cli).

#### Applying migrations

There are four ways to apply migrations:

- Scripting: running SQL script
- CLI tool: running `dotnet ef database update` command
- Application startup: calling `Database.Migrate()` method during application startup
- Migration bundles: running `./efbundle --verbose` command

##### Migration bundles

Migration bundles are single-file executables that can be used to apply migrations to a database. They address some of the shortcomings of the SQL script and command-line tools:

- Executing SQL scripts requires additional tools.
- The transaction handling and continue-on-error behavior of these tools are inconsistent and sometimes unexpected. This can leave your database in an undefined state if a failure occurs when applying migrations.
- Bundles can be generated as part of your CI process and easily executed later as part of your deployment process.
- Bundles can be executed without installing the .NET SDK or EF Tool (or even the .NET Runtime, when self-contained), and they don't require the project's source code.

## `dotnet format`

[↑ `dotnet format`](https://github.com/dotnet/format) is a code formatter that applies style preferences to a project or solution.

Preferences will be read from an `.editorconfig` file, if present, otherwise a default set of preferences will be used.

### Commands

| Command                             | Description                                                                                  |
| ----------------------------------- | -------------------------------------------------------------------------------------------- |
| `dotnet format whitespace`          | Fixes whitespace                                                                             |
| `dotnet format style`               | Runs code style analyzers                                                                    |
| `dotnet format analyzers`           | Runs third-party analyzers                                                                   |
| `dotnet format --verify-no-changes` | Formats but does not save. Returns a non-zero exit code if any files would have been changed |

### `.editorconfig` examples

[↑ runtime](https://github.com/dotnet/runtime/blob/main/.editorconfig).

[↑ dotnet format](https://github.com/dotnet/format/blob/main/.editorconfig).

[↑ Roslyn](https://github.com/dotnet/roslyn/blob/main/.editorconfig).

[↑ MessagePack](https://github.com/MessagePack-CSharp/MessagePack-CSharp/blob/master/.editorconfig).

[↑ aspnetcore](https://github.com/dotnet/aspnetcore/blob/main/.editorconfig).

### Code analyzers

#### Microsoft.VisualStudio.Threading.Analyzers

- [↑ Microsoft.VisualStudio.Threading.Analyzers](https://github.com/microsoft/vs-threading/blob/main/doc/analyzers/installation.md).

You may want to use this analyzer because of the problem with `Async` in interfaces: [↑ link 1](https://github.com/dotnet/roslyn/issues/40050), [↑ link 2](https://youtrack.jetbrains.com/issue/RSRP-486739).

##### Rule prefixes

| Analyzer                | Prefix |
| ----------------------- | ------ |
| StyleCop.Analyzers      | `SA`   |
| SonarAnalyzers.CSharp   | `S`    |
| NET.CodeAnalyzers       | `CA`   |
| Visual Studio.Analyzers | `IDE`  |

[↑ Severity levels](https://learn.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers).

### Other code formatters

[↑ Prettier-style line breaking](https://github.com/dotnet/format/issues/246).

[↑ CSharpier](https://csharpier.com), [↑ Rider plugin](https://plugins.jetbrains.com/plugin/18243-csharpier).

[↑ Overview of .NET source code analysis](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/overview).

[↑ Support Roslyn properties in editorconfig](https://youtrack.jetbrains.com/issue/RIDER-51400/Support-roslyn-properties-in-editorconfig).
