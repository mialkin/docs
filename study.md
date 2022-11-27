# Study list

## Table of contents

- [Study list](#study-list)
  - [Table of contents](#table-of-contents)
  - [Misc](#misc)
  - [C#](#c)
  - [ASP.NET](#aspnet)
  - [Design](#design)
  - [Programming](#programming)
  - [Databases](#databases)
  - [JavaScript](#javascript)
  - [Computer science](#computer-science)

## Misc

- [↑ TDD](https://www.youtube.com/watch?v=a7BvGBT0gFw)
- JavaScript promises, async/await
- DDD
- [↑ Introduction to Functional Programming in F#](https://docs.microsoft.com/en-us/dotnet/fsharp/introduction-to-functional-programming/)
- OpenTracing
- blue-green deployment
- MapReduce
- [↑ Lighthouse](https://developers.google.com/web/tools/lighthouse)
- See IL of `yield` [↑ What concrete type does 'yield return' return?](https://stackoverflow.com/questions/3454395/what-concrete-type-does-yield-return-return)
- [↑ Indices and ranges](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8#indices-and-ranges)
- async lock
- [↑ volatile (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/volatile)
- [↑ Weak reference](https://docs.microsoft.com/en-us/dotnet/api/system.weakreference?view=netcore-3.1)
- [↑ Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)
- Data structures time and space complexities
- [↑ ASP.NET Core Series: Performance Testing Techniques](https://www.youtube.com/watch?v=jn54CjePzs0)
- [↑ Higher-order function](https://en.wikipedia.org/wiki/Higher-order_function)
- [↑ First-class citizen](https://en.wikipedia.org/wiki/First-class_citizen)

## C#

- Access modifiers
- Closure in C# (пример взять из интервью). Найти более простое и понятное определение, чем то, что сейчас есть
- Expression tree https://tyrrrz.me/blog/expression-trees
- Finalizers
  - https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/finalizers
  - https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged
  - Сослаться на dispose-pattern, которй нужно описать на странице с IDisposable
  - https://stackoverflow.com/questions/20358401/throwing-exception-in-finalizer-to-enforce-dispose-calls
- `IDisposable`
  - https://stackoverflow.com/questions/7914423/struct-and-idisposable
  - https://ericlippert.com/2011/03/14/to-box-or-not-to-box/
  - Описать паттерн Dispose https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose
  - https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged
  - https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-disposeasync
- LINQ
  - Deffered execution and lazy evaluation https://docs.microsoft.com/en-us/dotnet/standard/linq/deferred-execution-lazy-evaluation
  - Примеры взять из интервью + https://stackoverflow.com/questions/7324033/what-are-the-benefits-of-a-deferred-execution-in-linq
- [↑ Local functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
- Memory managment
  - Managed heap
    - Small object heap (SOH)
- [↑ Grouping assertions with `AssertionScope`](https://ardalis.com/grouping-assertions-in-tests/)

## ASP.NET

- [↑ Caching](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration)
- [↑ Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters)
- [↑ Performance](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing)
- [↑ Servers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers)
- [↑ Upload files](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/file-uploads)

## Design

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
  - Event sourcing

## Programming

- [↑ Orleans](https://learn.microsoft.com/en-us/dotnet/orleans/overview)

## Databases

- [↑ Александр Шелёмин «Concurrency в базах данных»](https://www.youtube.com/watch?v=a6YzdDFzDl8)
- Cursor
- Execution plan
- Group by https://www.postgresql.org/docs/9.4/tutorial-agg.html
- Having
- Window function

## JavaScript

```js
for (var i = 0; i < 5; i++){
  setTimeout(function() { console.log(i); },  i * 1000);
}
```

## Computer science

Heap

https://www.geeksforgeeks.org/binary-heap/
https://en.wikipedia.org/wiki/Heap_(data_structure)
https://en.wikipedia.org/wiki/Binary_heap
https://stackoverflow.com/questions/25319305/why-does-a-binary-heap-has-to-be-a-complete-binary-tree
