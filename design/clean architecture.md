# Clean Architecture

- [Clean Architecture](#clean-architecture)
  - [Clean architecture rules](#clean-architecture-rules)
  - [Core project](#core-project)
  - [Infrastructure project](#infrastructure-project)
  - [Web project](#web-project)
  - [Shared kernel](#shared-kernel)
  - [Links](#links)

A **clean architecture** is just the latest in a series of names for the same loosely-coupled, dependency-inverted, domain-centric approach to organizing dependencies in an application. You will also find it named **hexagonal**, **ports and adapters**, or **onion architecture**.

There are two common approaches to dependency organization (arrows show direction of dependencies):

1. **N-tier** or **N-layer architecture**: UI →  business → data access → DB
2. **Clean architecture**: UI  → core ← infrastructure → DB

In clean architecture the UI and the infrastructure both depend on the core.

## Clean architecture rules

- All business rules and entities are modeled inside core project
- All dependencies flow toward the core project
- Inner projects define interfaces, outer projects implement them

> You should get familiar with [DDD patterns](ddd.md) before reading further.

## Core project

What belongs to core project:

- Interfaces
- Aggregates
- Entities
- Value objects
- Domain services. This is where logic lives that has to do with multiple entities or value objects and how they work with each other. You want to try and put as much behavior into your entities, aggregates and value objects as you can, but sometimes it doesn't belong to one of those and so you have domain services for that type of thing
- Custom domain exceptions. Something like `CustomerNotFoundException`
- Domain events
- Domain handlers. You could have handlers in other places as well, but the events themselves should usually be defined inside of core
- Specifications
- Validators. Like [↑ FluentValidation](https://github.com/FluentValidation/FluentValidation)
- Enums or [↑ SmartEnums](https://github.com/ardalis/SmartEnum)
- Custom guard classes. For example [↑ GuardClauses](https://github.com/ardalis/GuardClauses). Guard classes are simple validators you do to make sure that the system is in consistent state. You create custom ones that you reuse that apply to your domain model

`IAggregateRoot` interface is used to enforce persistence rule that says that we only wanna fetch and store aggregates as the whole, not individual entities. For example a `ToDoItem` is also an entity, but not an aggregate root, so it's not something you gonna persist on its own.

## Infrastructure project

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

## Web project

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

## Shared kernel

What belongs to shared kernel project:

- Base entity
- Base value object
- Base domain event
- Base specification
- Common interfaces
- Common authentication
- Common guards
- Common libraries
- Common exceptions
- DI
- Logging
- Validators

Shared kernel should have no infrastructure dependencies!

## Links

https://github.com/ardalis/CleanArchitecture