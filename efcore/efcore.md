# EF Core

## Installation

Install EF Core tools:

```zsh
dotnet tool install --global dotnet-ef
```

Before you can use the tools on a specific project, you'll need to add the `Microsoft.EntityFrameworkCore.Design` package to it.

## Updating

Update the tool tool using the following command:

```zsh
dotnet tool update --global dotnet-ef
```

## Commands

Command | Meaning
-|-
dotnet ef migrations add \<MigrationName> | Adds a new migration
dotnet ef migrations list | Lists available migrations
dotnet ef migrations remove | Removes the last migration
dotnet ef migrations script | Generates a SQL script from migrations
dotnet ef database drop | Deletes the database
dotnet ef database update | Updates the database to the last migration or to a specified migration
dotnet ef dbcontext info | Gets information about a DbContext type
dotnet ef dbcontext list | Lists available DbContext types
dotnet ef dbcontext scaffold | Generates code for a DbContext and entity types for a database. In order for this command to generate an entity type, the database table must have a primary key

## Links

[↑ .NET Core CLI
](https://docs.microsoft.com/en-us/ef/core/cli/dotnet)