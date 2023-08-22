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
  - [Разобрать](#разобрать)

## Tools

- Quartz.NET
  - <https://code-maze.com/schedule-jobs-with-quartz-net>
  - <https://www.quartz-scheduler.net/documentation/quartz-3.x/quick-start.html#nuget-package>
  - <https://blog.vfrz.fr/quartz-asp-net-core-3-0/>
- MongoDB
- Kafka
  - [↑ Error Handling Patterns for Apache Kafka Applications](https://www.confluent.io/blog/error-handling-patterns-in-kafka/)
  - [↑ Kafka producer delivery semantics](https://medium.com/@sdjemails/kafka-producer-delivery-semantics-be863c727d3f)
  - [↑ Can Your Kafka Consumers Handle a Poison Pill?](https://www.confluent.io/blog/spring-kafka-can-your-kafka-consumers-handle-a-poison-pill/)
- Postgres
  - [↑ Selecting for Share and Update in PostgreSQL](https://shiroyasha.io/selecting-for-share-and-update-in-postgresql.html)
  - [↑ SKIP LOCKED](https://www.2ndquadrant.com/en/blog/what-is-select-skip-locked-for-in-postgresql-9-5/)
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
- [↑ B-дерево](https://www.youtube.com/watch?v=WXXetwePSRk)
- [↑ Конспект доклада «Как стать классным спецом по бд» (HL2018, Data Egret, Илья Космодемьянский)](https://habr.com/ru/amp/publications/429508/)
- [↑ Covering indexes](https://www.red-gate.com/simple-talk/databases/sql-server/learn/using-covering-indexes-to-improve-query-performance/)
- [↑ SQL Server and Azure SQL index architecture and design guide](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-index-design-guide)
- [↑ `ConcurrencyCheck` attribute](https://www.entityframeworktutorial.net/code-first/concurrencycheck-dataannotations-attribute-in-code-first.aspx)
- [↑ NoSQL: виды, особенности и применение](https://cloud.yandex.ru/blog/posts/2022/10/nosql)

## Software architecture and design

- [↑ Architectural Katas](https://nealford.com/katas)
- [↑ Software Architecture in Practice (SEI Series in Software Engineering)](https://www.amazon.com/Software-Architecture-Practice-3rd-Engineering/dp/0321815734)
- [↑ Business Process Model and Notation](https://ru.wikipedia.org/wiki/BPMN)
  - [↑ BPMN tooling](https://bpmn.io)
  - [↑ Редактор Storm BPMN](https://stormbpmn.com)
  - [↑ Главный секрет BPMN: токены](https://www.youtube.com/watch?v=Gfx5atU3YDY)
  - [↑ Как и зачем добавлять BPM движок к своим микросервисам](https://www.youtube.com/watch?v=N2fxpoIHQqA)
- DDD
- OpenTracing
  - [↑ The Three Pillars of Observability](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/ch04.html)
  - [↑ Распределенный трейсинг OpenTelemetry вместо логирования всего подряд](https://dotnext.ru/talks/968f0bdc6f7a44b4b868c665d57dcaa5/)
- Architectural patterns
  - [↑ Event Sourcing](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)
    - [↑ EventSourcing.NetCore](https://github.com/oskardudycz/EventSourcing.NetCore)
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
- [↑ C4 model](https://en.wikipedia.org/wiki/C4_model)
  - <https://c4model.com>
- [↑ Почему авторизация сложно и причем здесь Занзибар?](https://www.youtube.com/watch?v=Tr5H8iG0FzI)
  - <https://zanzibar.academy>

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
  - [↑ Return the most specific type, accept the most generic type](https://enterprisecraftsmanship.com/posts/return-the-most-specific-type)
- [↑ Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [↑ GitOps](https://about.gitlab.com/topics/gitops)
- [↑ Есть ли жизнь на Go после C#?](https://habr.com/ru/companies/ozontech/articles/684422/)
- [↑ What are Functional and Non-Functional Requirements and How to Document These](https://enkonix.com/blog/functional-requirements-vs-non-functional)
- F.I.R.S.T. Principles
  - [↑ FIRST Principles as Solid Rules for Tests](https://dzone.com/articles/first-principles-solid-rules-for-tests)
  - [↑ F.I.R.S.T Principles of Unit Testing](https://github.com/tekguard/Principles-of-Unit-Testing)

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

- Memory managment
  - Small object heap (SOH)
  - [↑ The large object heap on Windows systems](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap)
  - [↑ CLRium #5: Курс "Garbage Collector"](https://www.youtube.com/watch?v=DVnmGW6964o)
  - [↑ Pinned Object Heap в .NET 5](https://habr.com/ru/post/593441/)
- [↑ dotnet-monitor](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-monitor)

## C&#35;

- Access modifiers
- See IL of `yield` [↑ What concrete type does 'yield return' return?](https://stackoverflow.com/questions/3454395/what-concrete-type-does-yield-return-return)
- Closure in C#
- [↑ Expression trees](https://tyrrrz.me/blog/expression-trees)
  - [↑ Expression Trees in C#](https://code-maze.com/csharp-expression-trees)
  - [↑ How to Build Dynamic Queries With Expression Trees in C#](https://code-maze.com/dynamic-queries-expression-trees-csharp/)
- IQueryable interface
- [↑ Specification pattern](https://deviq.com/design-patterns/specification-pattern)
  - <https://enterprisecraftsmanship.com/posts/specification-pattern-c-implementation>
  - <https://deviq.com/design-patterns/specification-pattern>
  - <https://designpatternsphp.readthedocs.io/en/latest/Behavioral/Specification/README.html>
  - <https://github.com/AdemCatamak/SpecificationPatternExample>
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
- [↑ How to Convert a String to a Span in C#](https://code-maze.com/csharp-how-to-convert-string-to-span)
- [↑ Async/await и механизм реализации в C# 5.0](https://habr.com/ru/articles/260217)
- [↑ Compile code programmatically by using C# compiler](https://learn.microsoft.com/en-us/troubleshoot/developer/visualstudio/csharp/language-compilers/compile-code-using-compiler)
- [↑ Reflection and Reflection.Emit in C#](https://www.c-sharpcorner.com/UploadFile/puranindia/reflection-and-reflection-emit-in-C-Sharp)
- Generate code from created expression tree for some mapper. Use generated code to map 2 objects
- [↑ Интернирование строк](https://learn.microsoft.com/en-us/dotnet/api/system.string.intern). Как отключить интернирование в коде?
- Заполнить список экземплярами классов и экземплярами структур и прогнать бенчмарки. Посмотреть где больше памяти отъедается и почему. Есть ли оверхед у структуры vs класс?
- [↑ CLR via C#](https://www.amazon.com/CLR-via-4th-Developer-Reference/dp/0735667454/) or [↑ The Book of the Runtime](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/README.md)
- Почему у структуры нет индекса синхронизации? [↑ Внутреннее устройство ссылочных типов C#](https://professorweb.ru/my/csharp/optimization/level2/2_1.php). Джеффри Рихтер
- Таблица виртуальных функций
- Как устроен `Task` и `ValueTask`
- `IAsyncDisposable`
- [↑ Утиная типизация в C#](https://habr.com/ru/articles/41377/)
- Monads
  - [↑ Monads explained in C#](https://mikhail.io/2016/01/monads-explained-in-csharp/)
  - [↑ The Maybe Monad (C#)](https://www.dotnetcurry.com/patterns-practices/1510/maybe-monad-csharp)

## ASP.NET

- [↑ Caching](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration)
- [↑ Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters)
  - [↑ Combining ASP.NET Core validation attributes with Value Objects](https://enterprisecraftsmanship.com/posts/combining-asp-net-core-attributes-with-value-objects/)
- [↑ Performance](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
- [↑ Routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing)
- [↑ Servers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers)
- [↑ ASP.NET Core Series: Performance Testing Techniques](https://www.youtube.com/watch?v=jn54CjePzs0)
- [↑ `.AddHttpLogging`](https://youtu.be/isrl_zJa1GY?t=987)
- [↑ FastEndpoints](https://fast-endpoints.com)
- [↑ Graceful shutdown](https://www.google.com/search?q=graceful+shutdown+asp.net+core&oq=Graceful+shutdown+asp&aqs=chrome.1.69i57j0i512j0i15i22i30j0i390i650l2.6447j0j7&sourceid=chrome&ie=UTF-8)

## Computer science

- [↑ Heap](<https://en.wikipedia.org/wiki/Heap_(data_structure)>), [\[2\]](https://en.wikipedia.org/wiki/Binary_heap)
- [↑ Binary Heap](https://www.geeksforgeeks.org/binary-heap)
- [↑ Why does a Binary Heap has to be a Complete Binary Tree?](https://stackoverflow.com/questions/25319305/why-does-a-binary-heap-has-to-be-a-complete-binary-tree)

## Web

- POST vs PUT vs PATCH
- [↑ Ant Design](https://ant.design)
- [↑ Material Design](https://m3.material.io)
- <https://html5book.ru/html-html5>
- <https://html5book.ru/css-css3>
- [↑ Sololearn](https://apps.apple.com/us/app/sololearn-learn-to-code-apps/id1210079064)
- [↑ StyleDrop: Text-To-Image Generation in Any Style](https://styledrop.github.io)
- [↑ Muse: Text-To-Image Generation via Masked Generative Transformers](https://muse-model.github.io)
- [↑ В чем разница между HTTP методами POST vs PUT vs PATCH?](https://dev-ed.ru/blog/http-post-put-patch/)
  - [↑ PUT vs PATCH & PUT vs POST](https://dev.to/mehmehmehlol/put-vs-patch-put-vs-post-56i9)

## Разобрать

- https://andrewlock.net/5-new-mvc-features-in-dotnet-7/
- https://moduscreate.com/blog/microservices-databases-migrations/
- https://www.postgresql.org/docs/current/rules-materializedviews.html
- https://www.theserverside.com/answer/Your-near-zero-downtime-microservices-migration-pattern
- https://www.oreilly.com/library/view/microservices-antipatterns-and/9781492042716/ch01.html
- https://habr.com/ru/companies/ozontech/articles/708274/
- https://highload.ru/spb/2022/abstracts/9317