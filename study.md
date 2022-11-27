# Study list

## Table of contents

- [Study list](#study-list)
  - [Table of contents](#table-of-contents)
  - [Tools](#tools)
  - [Databases](#databases)
  - [Software architecture and design](#software-architecture-and-design)
  - [Programming](#programming)
  - [JavaScript](#javascript)
  - [.NET](#net)
  - [C#](#c)
  - [ASP.NET](#aspnet)
  - [Computer science](#computer-science)

## Tools

- Kafka
  - [↑ Error Handling Patterns for Apache Kafka Applications](https://www.confluent.io/blog/error-handling-patterns-in-kafka/)
  - [↑ Kafka producer delivery semantics](https://medium.com/@sdjemails/kafka-producer-delivery-semantics-be863c727d3f)
- Postgres
  - [↑ Selecting for Share and Update in PostgreSQL](https://shiroyasha.io/selecting-for-share-and-update-in-postgresql.html)
- [↑ Serilog `LogContext`](https://github.com/serilog/serilog/wiki/Enrichment)

## Databases

- [↑ Александр Шелёмин «Concurrency в базах данных»](https://www.youtube.com/watch?v=a6YzdDFzDl8)
- Cursor
- Execution plan
- [Group by, Having](https://www.postgresql.org/docs/9.4/tutorial-agg.html)
- Window function

## Software architecture and design

- DDD
- OpenTracing
- Architectural patterns
  - [↑ Event Sourcing](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
  - [↑ Repository](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)
  - Microservices related patterns
    - Aggregator
    - [↑ Ambassador](https://docs.microsoft.com/en-us/azure/architecture/patterns/ambassador)
    - API Gateway
    - Circuit Breaker
    - [↑ Backends for Frontends](https://microservices.io/patterns/apigateway.html)
    - Retry
    - Service Layer
    - Service Locator
    - Sidecar
  - Unit of Work
- [↑ Disposable pattern](https://medium.com/@mypascal2000/disposable-patterns-ffa2145619e2)
  - [↑ Implement a Dispose method](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose)
- CAP theorem
- [↑ А какие виды CQRS вы знаете? Андрей Цветцих](https://www.youtube.com/watch?v=TnS6PwxHcLg)
- MapReduce
- Saga
  - [↑ Пример микросервисной архитектуры с Saga на MassTransit](https://habr.com/ru/post/664962/)
- [↑ Vertical Slice Architecture - Jimmy Bogard](https://www.youtube.com/watch?v=SUiWfhAhgQw)
- [↑ Distributed Tracing for Microservice Architecture](https://habr.com/ru/post/539022/)

## Programming

- [↑ Orleans](https://learn.microsoft.com/en-us/dotnet/orleans/overview)
- Blue-green deployment
- [↑ Higher-order function](https://en.wikipedia.org/wiki/Higher-order_function)
- [↑ First-class citizen](https://en.wikipedia.org/wiki/First-class_citizen)
- [↑ Денис Цветцих "Repository и UnitOfWork в 2020 году, must have или антипаттерн?"](https://www.youtube.com/watch?v=3yPpL1rEK9o)

## JavaScript

- JavaScript promises, async/await

```js
for (var i = 0; i < 5; i++) {
  setTimeout(function () {
    console.log(i);
  }, i * 1000);
}
```

## .NET

- Dotnet libraries
  - MediatR
    - [↑ You probably don't need MediatR](http://arialdomartini.github.io/mediatr)
    - [↑ You Probably Don't Need to Worry About MediatR](https://jimmybogard.com/you-probably-dont-need-to-worry-about-mediatr/)

## C&#35;

- Access modifiers
- See IL of `yield` [↑ What concrete type does 'yield return' return?](https://stackoverflow.com/questions/3454395/what-concrete-type-does-yield-return-return)
- Closure in C#
- [↑ Expression trees](https://tyrrrz.me/blog/expression-trees)
- [↑ Finalizers](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/finalizers)
  - [↑ Cleaning up unmanaged resources](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged)
  - [↑ Throwing exception in finalizer to enforce Dispose calls:](https://stackoverflow.com/questions/20358401/throwing-exception-in-finalizer-to-enforce-dispose-calls)
- `IDisposable`
  - [↑ Struct and IDisposable](https://stackoverflow.com/questions/7914423/struct-and-idisposable)
  - [↑ To box or not to box](https://ericlippert.com/2011/03/14/to-box-or-not-to-box/)
  - [↑ Implement a Dispose method](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose)
  - [↑ Implement a DisposeAsync method](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-disposeasync)
- LINQ
  - [↑ Deffered execution and lazy evaluation](https://docs.microsoft.com/en-us/dotnet/standard/linq/deferred-execution-lazy-evaluation)
  - [↑ What are the benefits of a Deferred Execution in LINQ?](https://stackoverflow.com/questions/7324033/what-are-the-benefits-of-a-deferred-execution-in-linq)
- [↑ Local functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
- Memory managment
  - Managed heap
    - Small object heap (SOH)
    - [↑ The large object heap on Windows systems](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap)
- [↑ Grouping assertions with `AssertionScope`](https://ardalis.com/grouping-assertions-in-tests/)
- [↑ volatile (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/volatile)
- [↑ Introduction to Functional Programming in F#](https://docs.microsoft.com/en-us/dotnet/fsharp/introduction-to-functional-programming/)
- async lock
- [↑ Weak reference](https://docs.microsoft.com/en-us/dotnet/api/system.weakreference?view=netcore-3.1)
- [↑ Immutable Collections](https://docs.microsoft.com/en-us/archive/msdn-magazine/2017/march/net-framework-immutable-collections)
  - [↑ How to use immutability in C#](https://www.infoworld.com/article/3564161/how-to-use-immutability-in-csharp.html)
  - [↑ A Use Case of Immutable Collections](https://codeburst.io/a-use-case-of-immutable-collections-dd558f614722)
  - [↑ C# Highlights: Immutable Collections](https://www.youtube.com/watch?v=_p-Z9gAYabM)
  - [↑ Immutable Collections in NET: Why and How?](https://www.youtube.com/watch?v=soyzikvjx3s)

## ASP.NET

- [↑ Caching](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration)
- [↑ Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters)
- [↑ Performance](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing)
- [↑ Servers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers)
- [↑ ASP.NET Core Series: Performance Testing Techniques](https://www.youtube.com/watch?v=jn54CjePzs0)

## Computer science

- [↑ Heap](<https://en.wikipedia.org/wiki/Heap_(data_structure)>), [\[2\]](https://en.wikipedia.org/wiki/Binary_heap)
- [↑ Binary Heap](https://www.geeksforgeeks.org/binary-heap)
- [↑ Why does a Binary Heap has to be a Complete Binary Tree?](https://stackoverflow.com/questions/25319305/why-does-a-binary-heap-has-to-be-a-complete-binary-tree)
