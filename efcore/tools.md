# EF Core tools

## Installation

```zsh
dotnet tool install --global dotnet-ef
```

Before you can use the tools on a specific project, you'll need to add the `Microsoft.EntityFrameworkCore.Design` package to it.

## Updating

```zsh
dotnet tool update --global dotnet-ef
```

## Commands

| Command                                 | Meaning                                                                                                                                                          |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dotnet ef database drop                 | Deletes the database                                                                                                                                             |
| dotnet ef database update               | Updates the database to the last migration or to a specified migration                                                                                           |
| dotnet ef dbcontext info                | Gets information about a DbContext type                                                                                                                          |
| dotnet ef dbcontext list                | Lists available DbContext types                                                                                                                                  |
| dotnet ef dbcontext scaffold            | Generates code for a DbContext and entity types for a database. In order for this command to generate an entity type, the database table must have a primary key |
| dotnet ef migrations add MIGRATION_NAME | Create new migration                                                                                                                                             |
| dotnet ef migrations list               | Lists available migrations                                                                                                                                       |
| dotnet ef migrations remove             | Removes the last migration                                                                                                                                       |
| dotnet ef migrations script             | Generates a SQL script from migrations                                                                                                                           |

## About migrations

When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database. You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist. Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.

This method of keeping the database in sync with the data model works well until you deploy the application to production. When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column. The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating a new database.
