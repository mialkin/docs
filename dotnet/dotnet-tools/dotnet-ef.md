# `dotnet-ef` tool

## Table of contents

- [`dotnet-ef` tool](#dotnet-ef-tool)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Base commands](#base-commands)
  - [Migrations](#migrations)
    - [Applying migrations](#applying-migrations)
      - [Migration bundles](#migration-bundles)

## Installation

```zsh
dotnet tool install --global dotnet-ef
```

Before you can use the tools on a specific project, you'll need to add the `Microsoft.EntityFrameworkCore.Design` package to it.

## Base commands

| Command                   | Meaning                                                                |
| ------------------------- | ---------------------------------------------------------------------- |
| dotnet ef database drop   | Deletes the database                                                   |
| dotnet ef database update | Updates the database to the last migration or to a specified migration |
| dotnet ef dbcontext info  | Gets information about a DbContext type                                |
| dotnet ef dbcontext list  | Lists available DbContext types                                        |

## Migrations

| Command                                 | Meaning                                |
| --------------------------------------- | -------------------------------------- |
| dotnet ef migrations add MIGRATION_NAME | Create new migration                   |
| dotnet ef migrations list               | Lists available migrations             |
| dotnet ef migrations remove             | Remove the last migration              |
| dotnet ef migrations script             | Generate a SQL script from migrations |

### Applying migrations

There are four ways to apply migrations:

- Scripting: running SQL script
- CLI tool: running `dotnet ef database update` command
- Application startup: calling `Database.Migrate()` method during application startup
- Migration bundles: running `./efbundle --verbose` command

#### Migration bundles

```bash
dotnet ef migrations bundle \
--startup-project src/Mialkin.Dictionary \
--project src/Mialkin.Dictionary.Infrastructure \
--self-contained \
--target-runtime linux-x64
```

Migration bundles are single-file executables that can be used to apply migrations to a database. They address some of the shortcomings of the SQL script and command-line tools:

- Executing SQL scripts requires additional tools.
- The transaction handling and continue-on-error behavior of these tools are inconsistent and sometimes unexpected. This can leave your database in an undefined state if a failure occurs when applying migrations.
- Bundles can be generated as part of your CI process and easily executed later as part of your deployment process.
- Bundles can be executed without installing the .NET SDK or EF Tool (or even the .NET Runtime, when self-contained), and they don't require the project's source code.
