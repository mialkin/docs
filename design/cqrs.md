# CQRS

**Command and Query Responsibility Segregation** or **CQRS** is a pattern that separates read and write operations for a data store.

CQRS requires commands to only modify data and queries to only read it.

The term CQRS was coined by [↑ Greg Young](https://www.youtube.com/watch?v=JHGkaShoyNs).

[↑ CQRS Documents](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf) by Greg Young.

## Table of contents

- [CQRS](#cqrs)
  - [Table of contents](#table-of-contents)
  - [What kinds of CQRS do you know?](#what-kinds-of-cqrs-do-you-know)
    - [Classical horizontal service architecture vs vertical architectures](#classical-horizontal-service-architecture-vs-vertical-architectures)
      - [Advantages of handlers over services](#advantages-of-handlers-over-services)
        - [A handler is much smaller than a service](#a-handler-is-much-smaller-than-a-service)
        - [A handler follows SRP principle](#a-handler-follows-srp-principle)
        - [A handler does not have code of other business operations](#a-handler-does-not-have-code-of-other-business-operations)
        - [A handler has less dependencies than a service](#a-handler-has-less-dependencies-than-a-service)
      - [Dependencies between business operations become explicit](#dependencies-between-business-operations-become-explicit)
    - [Will CQRS help with high load?](#will-cqrs-help-with-high-load)
    - [Evolving CQRS](#evolving-cqrs)
    - [Myths about CQRS](#myths-about-cqrs)
  - [CQRS and event sourcing](#cqrs-and-event-sourcing)
  - [Links](#links)
  - [Classical horizontal service organization](#classical-horizontal-service-organization)
  - [Vertical organization](#vertical-organization)

## What kinds of CQRS do you know?

Key points:

- CQRS is simple (пояснить)
- CQRS has many advantages compared to traditional service approach (пояснить)
- CQRS fits for different kinds of projects: you can use it for different stacks and in different domains (пояснить)

### Classical horizontal service architecture vs vertical architectures

There are two ways to organize application layer, aka Interactors in Clean Architecture:

- Classical horizontal service way
  - Services
- Vertical
  - CQRS handlers
  - Vertical slices

#### Advantages of handlers over services

##### A handler is much smaller than a service

The largest handlers can reach 500 lines of code. It's still several times less than number of lines in a service. Normally handler is much less than 500 lines of code. For a service to have from few hundreds to few thousands lines of code is an usual thing.

It's much easier to maintain a small handler with well-defined scope of responsibility than a big service.

##### A handler follows SRP principle

The SRP principle states that a module should have one, and only one, reason to change.  A handler is responsible for one business operation, so it complies with the SRP principle.e.

Architecture based on handlers expands by adding new handlers and not by rewriting existing handlers.

##### A handler does not have code of other business operations

Handlers are independent from each other. All the code of a business operation will be in one place, in one class — in a handler.

On the other hand operations can be spread across multiple services. One service can call other services. Operations inside a service get fragmented.

##### A handler has less dependencies than a service

It's a rare thing to see more than 5 dependencies inside of a handler. It's because handler has only dependencies it needs for implementing its business operation. For a service to have 10 or more dependencies is a usual thing.

#### Dependencies between business operations become explicit

In CQRS community there is an argument: is it ok to call a command or a query from inside handler or not? Spoiler: it's ok. BTW in services community there is no such an argument at all.

### Will CQRS help with high load?

### Evolving CQRS

### Myths about CQRS

## CQRS and event sourcing

> Everyone always talks about CQRS and event sourcing when, really, it's event sourcing and CQRS. When I first started teaching people about CQRS and event sourcing it was advantageous to teach them CQRS first, and then teach them event sourcing. You can use CQRS without event sourcing, but with event sourcing you must use CQRS.

[↑ Greg Young - CQRS and Event Sourcing - Code on the Beach 2014](https://youtu.be/JHGkaShoyNs?t=60).

> I have been talking about CQRS and event source subjects for a very, very long time. The first time I talked about this was in 2006 at Q Con San Francisco.

## Links

[↑ CQRS pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs).

[↑ А какие виды CQRS вы знаете?](https://www.youtube.com/watch?v=TnS6PwxHcLg).

## Classical horizontal service organization

With classical horizontal service architecture application layer organization we create a separate service for each entity in our domain and each public method of this service is a business operation: create order, get orders, etc:

```csharp
public class OrderService
{
    public Order CreateOrder()
    {
        // Create order here
    }

    public List<Order> GetOrders()
    {
        return _databaseContext.Orders;
    }
}
```

With this approach every entity will have its own service and all the services reside inside of one component:

```text
├── ApplicationServices.proj
    ├── OrderService.cs
    └── UserService.cs
```

## Vertical organization

With this approach for each business operation a command or a query is created with corresponding handler:

```csharp
public class GetOrdersQuery
{    
}

public class GetOrdersQueryHandler
{
    public List<Order> Handle(GetOrdersQuery request)
    {
        return _databaseContext.Orders.Find(request.Id);
    }
}
```

With this approach folder structure is going to be as follows:

```csharp
├── ApplicationLayer.csproj
    ├── Commands
    |   └── CreateOrder
    |       |── CreateOrderCommand.cs
    |       └── CreateOrderCommandHandler.cs
    ├── Dtos
    |    |── CreateOrderDto.cs
    |    └── GetOrdersDto.cs
    ├── Queries
    |   └── GetOrders
    |       |── GetOrdersQuery.cs
    |       └── GetOrdersQueryHandler.cs
    ├── Utils
        └── OrderMappingProfile.cs
```

Also we use the same model, for reading and writing data; model in DDD sense. Commands return values.

Described above approach is also know as [↑ Vertical Slice Architecture](https://jimmybogard.com/vertical-slice-architecture), the term coined by Jimmy Bogard.