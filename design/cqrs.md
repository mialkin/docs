# CQRS

**Command (and) Query Responsibility Segregation** or **CQRS** is a  a pattern that separates read and update operations for a data store.

## Table of contents

- [CQRS](#cqrs)
  - [Table of contents](#table-of-contents)
  - [What kinds of CQRS do you know?](#what-kinds-of-cqrs-do-you-know)
    - [Horizontal (classical) and vertical architectures](#horizontal-classical-and-vertical-architectures)
      - [Horizontal organization](#horizontal-organization)
      - [Vertical organization](#vertical-organization)
      - [Advantages of handlers over services](#advantages-of-handlers-over-services)
    - [What is CQRS](#what-is-cqrs)
    - [Will CQRS help with load growth?](#will-cqrs-help-with-load-growth)
    - [Evolving CQRS](#evolving-cqrs)
    - [Myths about CQRS](#myths-about-cqrs)
  - [CQRS and event sourcing](#cqrs-and-event-sourcing)
  - [Links](#links)

## What kinds of CQRS do you know?

Excerpts from the video [↑ А какие виды CQRS вы знаете?](https://www.youtube.com/watch?v=TnS6PwxHcLg).

Key points:

- CQRS is simple
- CQRS has many advantages compared to traditional service approach
- CQRS fits for different kinds of projects: you can use it for different stacks and in different domains

### Horizontal (classical) and vertical architectures

There are two ways to organize application layer, aka Interactors in Clean Architecture:

- Horizontal, classical way
  - Services
- Vertical
  - CQRS handlers
  - Vertical slices

#### Horizontal organization

With classical horizontal application layer organization we create a separate service for each entity in our domain and each public method of this service is a business operation: create order, get orders, etc:

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

#### Vertical organization

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

#### Advantages of handlers over services

1. A handler is much smaller than a service. The largest handlers can reach 500 lines of code. It's still several times less than number of lines in a service. Normally handler is much less than 500 lines of code. For a service to have from few hundreds to few thousands lines of code is an usual thing. Maintaining small handlers with a clear area of responsibility is much easier.
2. A handler does not have code of other business operations. Handlers are independent from each other. All the code of a business operation will be in one place, in one class — in a handler. Operations can be spread across different services. For example cloning of a complex entity can be spread across 7 services. One service can call other services. Operations inside a service get fragmented.
3. A handler follows SRP principle which states that class should have one reason to change. Architecture based on handlers expands by adding new handlers and not by rewriting existing handlers.
4. Handlers have less dependencies. Handler has only dependencies it needs for implementing one business operation. It's a rare thing to see more than 5 dependencies inside of a handler. For a service to have 10 or more dependencies is a normal thing.

### What is CQRS

CQRS is when commands can only change the data, queries can only read it.

The term CQRS was coined by Greg Young.

[↑ CQRS Documents by Greg Young](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf).

### Will CQRS help with load growth?

### Evolving CQRS

### Myths about CQRS

## CQRS and event sourcing

> Everyone always talks about CQRS and event sourcing when, really, it's event sourcing and CQRS. When I first started teaching people about CQRS and event sourcing it was advantageous to teach them CQRS first, and then teach them event sourcing. You can use CQRS without event sourcing, but with event sourcing you must use CQRS.

[↑ Greg Young - CQRS and Event Sourcing - Code on the Beach 2014](https://youtu.be/JHGkaShoyNs?t=60).

> I have been talking about CQRS and event source subjects for a very, very long time. The first time I talked about this was in 2006 at Q Con San Francisco.

## Links

- [↑ CQRS pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs)
- [↑ А какие виды CQRS вы знаете?](https://www.youtube.com/watch?v=TnS6PwxHcLg)