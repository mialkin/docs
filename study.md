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
  - [Web](#web)

## Tools

- Quartz.NET
  - <https://code-maze.com/schedule-jobs-with-quartz-net>
  - <https://www.quartz-scheduler.net/documentation/quartz-3.x/quick-start.html#nuget-package>
  - <https://blog.vfrz.fr/quartz-asp-net-core-3-0/>
- MongoDB
- Kafka
  - [↑ Error Handling Patterns for Apache Kafka Applications](https://www.confluent.io/blog/error-handling-patterns-in-kafka/)
  - [↑ Kafka producer delivery semantics](https://medium.com/@sdjemails/kafka-producer-delivery-semantics-be863c727d3f)
- Postgres
  - [↑ Selecting for Share and Update in PostgreSQL](https://shiroyasha.io/selecting-for-share-and-update-in-postgresql.html)
  - [↑ Group by, Having](https://www.postgresql.org/docs/9.4/tutorial-agg.html)
- [↑ Serilog `LogContext`](https://github.com/serilog/serilog/wiki/Enrichment)
- [↑ Refit](https://github.com/reactiveui/refit) and [↑ RestSharp](https://github.com/restsharp/RestSharp)

## Databases

- [↑ «Concurrency в базах данных»](https://www.youtube.com/watch?v=a6YzdDFzDl8)
- Cursor
- Execution plan
- Window function
- [↑ Apache Cassandra](https://cassandra.apache.org/_/cassandra-basics.html)
- [↑ Performance benchmarks of PostgreSQL .NET with Npgsql, Dapper, and Entity Framework Core](https://michaelscodingspot.com/npgsql-dapper-efcore-performance/)

## Software architecture and design

- [↑ Architectural Katas](https://nealford.com/katas)
- [↑ Software Architecture in Practice (SEI Series in Software Engineering)](https://www.amazon.com/Software-Architecture-Practice-3rd-Engineering/dp/0321815734)
- DDD
- OpenTracing
  - [↑ The Three Pillars of Observability](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/ch04.html)
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
      - [↑ Implement HTTP call retries with exponential backoff with IHttpClientFactory and Polly policies](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
    - Service Layer
    - Service Locator
    - Sidecar
  - Unit of Work
- [↑ Disposable pattern](https://medium.com/@mypascal2000/disposable-patterns-ffa2145619e2)
  - [↑ Implement a Dispose method](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose)
- CAP theorem
- MapReduce
- Saga
  - [↑ Пример микросервисной архитектуры с Saga на MassTransit](https://habr.com/ru/post/664962/)
- [↑ Vertical Slice Architecture - Jimmy Bogard](https://www.youtube.com/watch?v=SUiWfhAhgQw)
- [↑ Distributed Tracing for Microservice Architecture](https://habr.com/ru/post/539022/)
- [↑ Concurrent Processing in .NET 6 with System.Threading.Channels](https://itnext.io/concurrent-processing-in-net-6-with-system-threading-channels-bonus-interval-trees-441b7539b5d1)
- CQRS
  - CQRS может без Event sourcing. Event sourcing без CQRS не может. CQRS — это когда только Command может менять модель (модель в смысле DDD, то есть модель какой-то предметной области), Query может только ее читать. [↑ А какие виды CQRS вы знаете?](https://www.youtube.com/watch?v=TnS6PwxHcLg)
- [↑ C4 model](https://en.wikipedia.org/wiki/C4_model)
  - <https://c4model.com>

## Programming

- [↑ Orleans](https://learn.microsoft.com/en-us/dotnet/orleans/overview)
- Blue-green deployment
- [↑ Higher-order function](https://en.wikipedia.org/wiki/Higher-order_function)
- [↑ First-class citizen](https://en.wikipedia.org/wiki/First-class_citizen)
- [↑ "Repository и UnitOfWork в 2020 году, must have или антипаттерн?"](https://www.youtube.com/watch?v=3yPpL1rEK9o)
- [↑ Domain-Driven Refactoring - Jimmy Bogard - NDC London 2022](https://www.youtube.com/watch?v=f64tZ90Dntg)
- [↑ Тестируем микросервисы правильно](https://www.youtube.com/watch?v=yj3sndjLHEA)
- [↑ Чистая архитектура на практике](https://www.youtube.com/watch?v=Bd83nPK_K3U)
- [↑ Generic types are for arguments, specific types are for return values](https://enterprisecraftsmanship.com/posts/generic-types-arguments-specific-types-return-values/)
- [↑ Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [↑ GitOps](https://about.gitlab.com/topics/gitops)

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
  - [↑ Dependabot](https://github.com/dependabot)
    - [↑ Integrating Dependabot into your .NET Core project on GitHub](https://medium.com/@nickfane/integrating-dependabot-into-your-net-core-project-on-github-3e3024bd3394)
  - MediatR
    - [↑ You probably don't need MediatR](http://arialdomartini.github.io/mediatr)
    - [↑ You Probably Don't Need to Worry About MediatR](https://jimmybogard.com/you-probably-dont-need-to-worry-about-mediatr/)
- Memory managment
  - Small object heap (SOH)
  - [↑ The large object heap on Windows systems](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap)
  - [↑ CLRium #5: Курс "Garbage Collector"](https://www.youtube.com/watch?v=DVnmGW6964o)
  - [↑ Pinned Object Heap в .NET 5](https://habr.com/ru/post/593441/)

## C&#35;

- Access modifiers
- See IL of `yield` [↑ What concrete type does 'yield return' return?](https://stackoverflow.com/questions/3454395/what-concrete-type-does-yield-return-return)
- Closure in C#
- [↑ Expression trees](https://tyrrrz.me/blog/expression-trees)
- IQueryable interface
- [↑ Specification pattern](https://deviq.com/design-patterns/specification-pattern)
  - <https://enterprisecraftsmanship.com/posts/specification-pattern-c-implementation>
  - <https://deviq.com/design-patterns/specification-pattern>
  - <https://en.wikipedia.org/wiki/Specification_pattern>
  - <https://medium.com/c-sharp-progarmming/specification-design-pattern-c814649be0ef>
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
- [↑ Grouping assertions with `AssertionScope`](https://ardalis.com/grouping-assertions-in-tests/)
- [↑ volatile (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/volatile)
- [↑ Introduction to Functional Programming in F#](https://docs.microsoft.com/en-us/dotnet/fsharp/introduction-to-functional-programming/)
- async lock
- [↑ `AsyncLocal<T>`](https://code-maze.com/charp-difference-between-returning-and-awaiting-a-task/)
- [↑ How Async/Await Really Works in C#](https://devblogs.microsoft.com/dotnet/how-async-await-really-works/)
- [↑ Weak reference](https://docs.microsoft.com/en-us/dotnet/api/system.weakreference?view=netcore-3.1)
- [↑ Immutable Collections](https://docs.microsoft.com/en-us/archive/msdn-magazine/2017/march/net-framework-immutable-collections)
  - [↑ How to use immutability in C#](https://www.infoworld.com/article/3564161/how-to-use-immutability-in-csharp.html)
  - [↑ A Use Case of Immutable Collections](https://codeburst.io/a-use-case-of-immutable-collections-dd558f614722)
  - [↑ C# Highlights: Immutable Collections](https://www.youtube.com/watch?v=_p-Z9gAYabM)
  - [↑ Immutable Collections in NET: Why and How?](https://www.youtube.com/watch?v=soyzikvjx3s)
- [↑ Clean Up Your Client to Business Logic Relationship With a Result Pattern (C#)](https://alexdunn.org/2019/02/25/clean-up-your-client-to-business-logic-relationship-with-a-result-pattern-c/)
- [↑ Asynchronous TCP/IP, Part 1](https://www.youtube.com/watch?v=RJOdB5ly7jk)
- [↑ Stephen Cleary — Asynchronous streams](https://www.youtube.com/watch?v=-Tq4wLyen7Q)

## ASP.NET

- [↑ Caching](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration)
- [↑ Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters)
  - [↑ Combining ASP.NET Core validation attributes with Value Objects](https://enterprisecraftsmanship.com/posts/combining-asp-net-core-attributes-with-value-objects/)
- [↑ Performance](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing)
- [↑ Servers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers)
- [↑ ASP.NET Core Series: Performance Testing Techniques](https://www.youtube.com/watch?v=jn54CjePzs0)
- Feature flags
  - [↑ Tutorial: Use feature flags in an ASP.NET Core app](https://learn.microsoft.com/en-us/azure/azure-app-configuration/use-feature-flags-dotnet-core?tabs=core5x)
  - [↑ Feature flags](https://docs.gitlab.com/ee/operations/feature_flags.html)
  - [↑ Unleash, an open source feature management solution](https://github.com/Unleash/unleash)

## Computer science

- [↑ Heap](<https://en.wikipedia.org/wiki/Heap_(data_structure)>), [\[2\]](https://en.wikipedia.org/wiki/Binary_heap)
- [↑ Binary Heap](https://www.geeksforgeeks.org/binary-heap)
- [↑ Why does a Binary Heap has to be a Complete Binary Tree?](https://stackoverflow.com/questions/25319305/why-does-a-binary-heap-has-to-be-a-complete-binary-tree)

## Web

- [↑ Ant Design](https://ant.design)
- [↑ Material Design](https://m3.material.io)
- <https://html5book.ru/html-html5>
- <https://html5book.ru/css-css3>
- [↑ Sololearn](https://apps.apple.com/us/app/sololearn-learn-to-code-apps/id1210079064)
