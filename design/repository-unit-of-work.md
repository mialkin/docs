# Repository, unit of work

The *repository* and the *unit of work* patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.

## Repository

The **repository** is a design pattern that provides an abstraction layer between the application's business logic and the data access code.

## Unit of work

The **unit of work** is a design pattern that maintains a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems.

[↑ Unit of Work](https://www.martinfowler.com/eaaCatalog/unitOfWork.html).

[↑ Unit of Work in ASP.NET Core](https://www.youtube.com/watch?v=D_clPDWYpgs).

## .NET `DbContext` class

The lifetime of a `DbContext` begins when the instance is created and ends when the instance is disposed. A `DbContext` instance is designed to be used for a *single* unit of work. This means that the lifetime of a `DbContext` instance is usually very short.

[↑ DbContext Lifetime, Configuration, and Initialization](https://learn.microsoft.com/en-us/ef/core/dbcontext-configuration/).

A `DbContext` instance represents a combination of the Unit Of Work and Repository patterns such that it can be used to query from a database and group together changes that will then be written back to the store as a unit.

[↑ DbContext class](https://learn.microsoft.com/en-us/dotnet/api/system.data.entity.dbcontext).
