# Clean Architecture

A **clean architecture**, formely known as **onion architecture**, **hexagonal architecture**, **ports and adapters**, is a domain-centric approach to organizing dependencies in an application.

There are two common approaches to dependency organization (arrows show direction of dependencies):

1. **N-tier** or **N-layer**: UI →  business → data access → DB.
2. **clean architecture**: UI  → core (business) ← infrastructure (including data access) → DB.

In clean architecture the UI and the infrastructure both depend on the core.

Clean architecture rules:

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
- Domain services. This is where logic lives that has to do with multiple entities or value objects and how they work with each other. You want to try and put as much behavior into your entities, aggregates and value objects as you can, but sometimes it doesn't belong to one of those and so you have domain services for that type of thing.
- Custom domain exceptions. Something like `CustomerNotFoundException`
- Domain events
- Domain handlers. You could have handlers in other places as well, but the events themselves should usually be defined inside of core
- Specifications
- Validators. Like [↑ FluentValidation](https://github.com/FluentValidation/FluentValidation)
- Enums or [↑ SmartEnums](https://github.com/ardalis/SmartEnum)
- Custom guard classes. For example [↑ GuardClauses](https://github.com/ardalis/GuardClauses). Guard classes are simple validators you do to make sure that the system is in consistent state. You create custom ones that you reuse that apply to your domain model.
