# Clean architecture

A **clean architecture** is just the latest in a series of names for the same loosely-coupled, dependency-inverted, domain-centric approach to organizing dependencies in an application. You will also find it named [↑ **hexagonal**](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)), **ports and adapters**, or **onion architecture**.

There are two common approaches to dependency organization, arrows show direction of dependencies:

1. **N-tier/N-layer architecture**: UI → business layer → data access layer → database
2. **Clean architecture**: UI → core ← infrastructure (including data access) → database

In clean architecture the UI and the infrastructure both depend on the core.

Clean architecture rules:

- All business rules and entities are modeled inside the core project
- All dependencies flow towards the core project
- Inner projects define interfaces, outer projects implement them

## Table of Contents

- [Clean architecture](#clean-architecture)
  - [Table of Contents](#table-of-contents)
  - [Core Project](#core-project)
  - [Infrastructure Project](#infrastructure-project)
  - [Web Project](#web-project)
  - [Shared Kernel](#shared-kernel)
  - [GitHub Repository](#github-repository)
  - [YouTube Videos](#youtube-videos)

## Core Project

What belongs to core project:

- Interfaces
- [↑ Aggregates](https://deviq.com/domain-driven-design/aggregate-pattern)
- [↑ Entities](https://deviq.com/domain-driven-design/entity)
- [↑ Value objects](https://deviq.com/domain-driven-design/value-object)
- Domain services. This is where logic lives that has to do with multiple entities or value objects and how they work with each other. You want to try and put as much behavior into your entities, aggregates and value objects as you can, but sometimes it doesn't belong to one of those and so you have domain services for that type of thing
- Custom domain exceptions. Something like `CustomerNotFoundException`
- Domain events
- Domain event handlers. You could have handlers in other places as well, but the events themselves should usually be defined inside of core
- [↑ Specifications](https://deviq.com/design-patterns/specification-pattern)
- Validators. Like [↑ FluentValidation](https://github.com/FluentValidation/FluentValidation)
- Enums
- Custom [↑ guard clauses](https://github.com/ardalis/GuardClauses). Guard clauses are simple validators you do to make sure that the system is in consistent state. You create custom ones that you reuse that apply to your domain model

The Core project is the center of the clean architecture design, and all other project dependencies should point towards it. As such, it has very few external dependencies.

## Infrastructure Project

What belongs to infrastructure project:

- Repositories
- `DbContext` classes
- Cached repositories
- API clients
- File system accessors
- Cloud storage accessor
- Emailing implementations
- SMS implementations
- Other services
- Other interfaces. Those do not use domain models for their parameters and return types. For instance, if you are using some SDK and it's returning some of its type you don't want to put it in the core, because now core will have dependency on SDK's type

Most of your application's dependencies on external resources should be implemented in classes defined in the Infrastructure project. These classes should implement interfaces defined in Core.

## Web Project

What belongs to web project:

- API endpoints
- Razor pages
- Controllers
- Views
- API models
- ViewModels
- Filters
- Model binders
- Composition root. This is where the configuration of services is done: this interface is served by this actual implementation class
- Other services
- Other interfaces. The same rules apply as with infrastructure's other interfaces and services

The Web project is an entry point of the application.

## Shared Kernel

What belongs to shared kernel project:

- Base entity
- Base value object
- Base domain event
- Base specification
- Common interfaces
- Common authentication
- Common [↑ guard clauses](https://github.com/ardalis/GuardClauses)
- Common libraries
- Common exceptions
- DI
- Logging
- Common validators

Shared Kernel should have no infrastructure dependencies!

It's recommended to create a separate Shared Kernel project and solution if you will require sharing code between multiple bounded contexts. It's further recommended to be published as a NuGet package, most likely privately within your organization, and referenced as a NuGet dependency by those projects that require it.

## GitHub Repository

[↑ Clean Architecture](https://github.com/ardalis/CleanArchitecture).

## YouTube Videos

[↑ Clean Architecture with ASP.NET Core 6](https://www.youtube.com/watch?v=lkmvnjypENw).

[↑ Clean Architecture with ASP.NET Core 3.0 by Jason Taylor](https://www.youtube.com/watch?v=dK4Yb6-LxAk).

[↑ Денис Цветцих: «Ты можешь лучше, чем программисты Microsoft»](https://youtu.be/IVm-qTyDor4?t=744).
