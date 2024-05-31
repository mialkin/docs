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
- Hangfire
- [↑ MassTransit](https://www.youtube.com/@PhatBoyG/videos)
- [↑ MongoDB](https://www.mongodb.com)
- Kafka
  - [↑ Error Handling Patterns for Apache Kafka Applications](https://www.confluent.io/blog/error-handling-patterns-in-kafka/)
  - [↑ Kafka producer delivery semantics](https://medium.com/@sdjemails/kafka-producer-delivery-semantics-be863c727d3f)
  - [↑ Can Your Kafka Consumers Handle a Poison Pill?](https://www.confluent.io/blog/spring-kafka-can-your-kafka-consumers-handle-a-poison-pill/)
  - [↑ Apache Kafka: устройство, плюсы, минусы и границы применимости](https://www.youtube.com/watch?v=eHxOX3YYxGc)
- [↑ Serilog `LogContext`](https://github.com/serilog/serilog/wiki/Enrichment)
- [↑ ASP.NET Core SignalR Tutorials](https://www.youtube.com/playlist?list=PLOeFnOV9YBa7nzzuXnThdfsyY06AuCP5V)
- [↑ Refit](https://github.com/reactiveui/refit) and [↑ RestSharp](https://github.com/restsharp/RestSharp) and [↑ RestEase](https://github.com/canton7/RestEase)
- [↑ pact-net](https://github.com/pact-foundation/pact-net)
  - [↑ Introduction to contract testing with Pactflow](https://www.youtube.com/playlist?list=PLwy9Bnco-IpfZ72VQ7hce8GicVZs7nm0i)
- Vault secrets webhook
  - [↑ Inject secrets directly into Pods from Vault revisited](https://banzaicloud.com/blog/inject-secrets-into-pods-vault-revisited/)
  - [↑ Kubernetes Meetup Yekaterinburg 001](https://www.youtube.com/watch?v=YUJRVu66ld8&t=1953s)
  - Try [↑ Qodana](https://www.jetbrains.com/qodana).
- [↑ Apache Airflow](https://airflow.apache.org)

## Databases

- [↑ EAV, Entity–attribute–value model](https://en.wikipedia.org/wiki/Entity%E2%80%93attribute%E2%80%93value_model)
- [↑ Concurrency Tokens](https://www.npgsql.org/efcore/modeling/concurrency.html)
- [↑ «Concurrency в базах данных»](https://www.youtube.com/watch?v=a6YzdDFzDl8)
- Cursor
  - [↑ What are the benefits of using database cursor?](https://stackoverflow.com/questions/3861558/what-are-the-benefits-of-using-database-cursor)
- Execution plan
- [↑ Apache Cassandra](https://cassandra.apache.org/_/cassandra-basics.html)
- [↑ Performance benchmarks of PostgreSQL .NET with Npgsql, Dapper, and Entity Framework Core](https://michaelscodingspot.com/npgsql-dapper-efcore-performance/)
- [↑ Конспект доклада «Как стать классным спецом по бд» (HL2018, Data Egret, Илья Космодемьянский)](https://habr.com/ru/amp/publications/429508/)
- [↑ SQL Server and Azure SQL index architecture and design guide](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-index-design-guide)
- [↑ `ConcurrencyCheck` attribute](https://www.entityframeworktutorial.net/code-first/concurrencycheck-dataannotations-attribute-in-code-first.aspx)
  - <https://metanit.com/sharp/entityframeworkcore/2.11.php>
- [↑ NoSQL: виды, особенности и применение](https://cloud.yandex.ru/blog/posts/2022/10/nosql)
- Define what full text search is, how it works and define [↑ inverted index](https://en.wikipedia.org/wiki/Inverted_index)
- Как можно вернуть данные из T-SQL-процедуры?
- T-SQL table variables vs temporary tables
- Create a deadlock. "Forget" on purpose to commit transaction and see what happens. Try using different isolation levels. <https://www.sqlservercentral.com/forums/topic/what-happens-with-uncommitted-transactions>. Are there any built-in timeouts for transaction to commit?
- [↑ drop table vs truncate table](https://www.google.com/search?q=drop+table+vs+truncate+table)
- Преимущества и недостатки хранимых процедур и триггеров
- [↑ Nested loop join](https://www.google.com/search?q=nested+loop+join)
- Какие недостатки есть у индексов, какое негативное влияние они оказывают
- Как асинхронно создать индекс
- Умение делать оптимистические и пессимистические блокировки
- Шардирование и секционирование и какие проблемы эти техники решают
- Схема репликации master-slave
- Устройство индексов (B-tree, LSM-Tree.)
  - [↑ B-tree индексы в базах данных на примере PostgreSQL](https://www.youtube.com/watch?v=mnEU2_cwE_s)
- Как делать префиксный и полнотекстовый поиск в базе, как устроены индексы в этом случае
- Что такое Materialized view
- Зачем нужно денормализовывать данные
- Что такое курсоры
- Привести пример аналитических функций и объяснить как их использовать
- Что такое стратегии JOIN (LOOP | HASH | MERGE JOIN)
- Database locks
  - Когда применять каждый тип блокировки
  - Умеет реализовывать распределенную блокировку через БД
- Transactions
  - Что такое распределенные транзакции и как их делать (Open XA, 2 phase commit)
- Scaling patterns
  - Когда нужно использовать секционирование и шардирование
  - Как выбрать ключ партиционирования
  - Как выбрать ключ шардирования
  - Что такое локальный и глобальный индекс, какие они несут преимущества и недостатки
  - Разные виды репликаций (master-slave, master-master, событийная, асинхронная, синхронная, полусинхронная)
  - Сделать пагинацию в случаях различных join-ов
  - Стратегии разрешения конфликтов репликации
  - Подход Change Data Capture, способы его реализации
- Postgres
  - [↑ Selecting for Share and Update in PostgreSQL](https://shiroyasha.io/selecting-for-share-and-update-in-postgresql.html)
  - [↑ SKIP LOCKED](https://www.2ndquadrant.com/en/blog/what-is-select-skip-locked-for-in-postgresql-9-5/)
  - [↑ Group by, Having](https://www.postgresql.org/docs/9.4/tutorial-agg.html)
  - Разобрать задачи с HAVING

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
    - [↑ Event Sourcing: как мы ускорили SQL проекции с 7 часов до 7 минут](https://highload.ru/moscow/2023/abstracts/10083)
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
- Saga
  - [↑ Пример микросервисной архитектуры с Saga на MassTransit](https://habr.com/ru/post/664962/)
- [↑ Vertical Slice Architecture - Jimmy Bogard](https://www.youtube.com/watch?v=SUiWfhAhgQw)
- [↑ Distributed Tracing for Microservice Architecture](https://habr.com/ru/post/539022/)
- [↑ Concurrent Processing in .NET 6 with System.Threading.Channels](https://itnext.io/concurrent-processing-in-net-6-with-system-threading-channels-bonus-interval-trees-441b7539b5d1)
- [↑ C4 model](https://en.wikipedia.org/wiki/C4_model)
  - <https://c4model.com>
- [↑ Почему авторизация сложно и причем здесь Занзибар?](https://www.youtube.com/watch?v=Tr5H8iG0FzI)
  - <https://zanzibar.academy>
- [↑ Cloud Desing Patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/)
- [↑ Distributed Systems lecture series](https://www.youtube.com/playlist?list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB)
- [↑ AsyncAPI & event-driven architecture](https://www.asyncapi.com/)
- [↑ Lock-free коллекции в .NET 6](https://www.youtube.com/watch?v=-fTrew8atpk)
  - [↑ Посмотреть доклады по .NET на канале IT и т.д.](https://www.youtube.com/@IT_i_td/videos)
- Define what loosely-coupled monolith is and what is distributed monolith https://codeopinion.com/loosely-coupled-monolith/

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
    [↑ HighLoad++](https://tinkoff.ktalk.ru/recordings/Z11VfgwKXpCHLRQrJs7u)

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
  - [↑ The large object heap on Windows systems](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap)
  - [↑ Оптимизация сборки мусора в высоконагруженном .NET сервисе](https://habr.com/ru/articles/452298/)
  - [↑ CLRium #5: Курс "Garbage Collector"](https://www.youtube.com/watch?v=DVnmGW6964o)
  - [↑ Memory leaks](https://github.com/sebastienros/memoryleak)
  - Learn dotMemory and dotTrace
  - `WaitForPendingFinalizers`
  - [↑ When should I use `GC.SuppressFinalize()`?](https://stackoverflow.com/questions/151051/when-should-i-use-gc-suppressfinalize)
  - [↑ Finalizers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/finalizers)
- [↑ dotnet-monitor](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-monitor)
- [↑ Pact NET. Consumer driven contract testing](https://github.com/pact-foundation/pact-net)
- [↑ Allure framework](https://docs.qameta.io/allure)
  - [↑ Allure.XUnit](https://github.com/Tinkoff/Allure.XUnit)
- Data masking
  - [↑ Destructurama.Attributed](https://github.com/destructurama/attributed)
  - [↑ Serilog.Enrichers.Sensitive](https://github.com/serilog-contrib/Serilog.Enrichers.Sensitive/tree/master)
  - [↑ What is data masking?](https://aws.amazon.com/what-is/data-masking/)
- Consider implementing in a project [The NEW Way of Validating Settings in .NET 8](https://www.youtube.com/watch?v=mO0fwvnnzbU)?

## C&#35;

- [↑ Access modifiers](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/access-modifiers)
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
- [↑ Local functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions)
- [↑ Grouping assertions with `AssertionScope`](https://ardalis.com/grouping-assertions-in-tests/)
- [↑ volatile (C# Reference)](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/volatile)
- async lock
- [↑ Writing async/await from scratch in C# with Stephen Toub](https://www.youtube.com/watch?v=R-z2Hv-7nxk).
- [↑ How Async/Await Really Works in C#](https://devblogs.microsoft.com/dotnet/how-async-await-really-works/)
- [↑ Why can't I use the 'await' operator within the body of a `lock` statement?](https://stackoverflow.com/questions/7612602/why-cant-i-use-the-await-operator-within-the-body-of-a-lock-statement)
- [↑ Smallest .Net ref type is 12 bytes (or why you should consider using value types)](https://theburningmonk.com/2015/07/smallest-net-ref-type-is-12-bytes-or-why-you-should-consider-using-value-types/)
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
- Just-In-Time IL code generation
- Try use [↑ ILSpy built with Avalonia](https://github.com/icsharpcode/AvaloniaILSpy)
- [↑ Reflection and Reflection.Emit in C#](https://www.c-sharpcorner.com/UploadFile/puranindia/reflection-and-reflection-emit-in-C-Sharp)
- Generate code from created expression tree for some mapper. Use generated code to map 2 objects
- [↑ Пишем код, когда пишем код: сорс-генераторы](https://habr.com/ru/companies/tinkoff/articles/766916/)
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
  - [↑ "Maybe" monad through async/await in C# (No Tasks!)](https://habr.com/ru/articles/458692/)
- [↑ Autofac](https://github.com/autofac/Autofac)
- Reflection. Create a separate repository with simple examples of getting class methods and class method's attributes
- На странице interface в репозитории docs разобрать какие типы данных могут реализовывать интерфейсы
- Приведение типов и вызов виртуальных и абстрактных методов. Создать отдельный репозиторий для задач с интервью?
- Implement merge sort using TPL
- Выписать XoR, побитовый сдвиг влево/вправо и прочие операции в CS-секцию с примерами реализации на C#
- Бесконечный вызов рекурсивной функции. Есть ли минусы? Будет ли происходить переполнение стека?
- Что нужно чтобы использовать using и await (GetEnumerator, GetAwaiter)?
- Cohesion <https://www.youtube.com/watch?v=r1Ih_nV79eE>, <https://ru.wikipedia.org/wiki/Связность_(программирование)>
- Чем лямбда отличается от обычной функции?
- try { return 1; } finally { return 2; } // Cannot jump out of the finally block
- Наследование в статических классах. Почему его нет?
- Разобрать .Cast<>, как реализован?
- Почему разработчики языка разделили память на стек и кучу?
- Написать бинарное дерево на C#
- Написать стек, двусвязный и односвязный список на C#
- [↑ Async HTTP Server](https://jonlabelle.com/snippets/view/csharp/async-http-server-and-client)
- Process asynchronous tasks as they complete (C#)
  - <https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete>
  - <https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/>
  - <https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks>
- [↑ Concurrency Visualizer](https://docs.microsoft.com/en-us/visualstudio/profiling/concurrency-visualizer)
- Разобрать C# AggregateException, посоздавать таких исключений, применить метод AggregateException.Flatten (страница 32 Стивена Клири)
- [↑ Adventure in learning Go for a C# dev! - Ken Faulkner](https://www.youtube.com/watch?v=0ghHBPNe6g4)
- Как реализованы дженерики в C#?
- [↑ MessagePack](https://github.com/neuecc/MessagePack-CSharp)
  - MessagePack vs Apache Avro
- .NET channels <https://www.youtube.com/watch?v=gT06qvQLtJ0>, <https://www.youtube.com/watch?v=E0Ld7ZgE4oY>
- What is the difference between `dynamic` and `object` keywords
- [↑ Why C# structs do no support inheritance](https://pragmateek.com/why-c-structs-do-no-support-inheritance/)
- Use [↑ `HashCode.Combine`](https://stackoverflow.com/a/51716512/1833895)
- EF Core
  - [↑ Eager, Lazy and Explicit Loading with Entity Framework Core](https://blog.jetbrains.com/dotnet/2023/09/21/eager-lazy-and-explicit-loading-with-entity-framework-core/)
  - Implement one-to-one, one-to-many, many-to-many in <https://github.com/mialkin/postgres>
- [↑ `SearchValues<T>`](https://www.youtube.com/watch?v=IzDMg916t98)
- [↑ Tutorial: Write your first analyzer and code fix](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/tutorials/how-to-write-csharp-analyzer-code-fix)
  - [↑ Menees.Analyzers](https://github.com/menees/Analyzers/tree/master)
- [↑ Inline и throw](https://habr.com/ru/articles/722374/)

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
- [↑ Introducing the Identity API endpoints](https://andrewlock.net/exploring-the-dotnet-8-preview-introducing-the-identity-api-endpoints/)
- [↑ Scheduled or Delayed Messages in Wolverine](https://jeremydmiller.com/2023/09/06/scheduled-or-delayed-messages-in-wolverine/)
  - [↑ Wolverine](https://wolverine.netlify.app/)
- [↑ Session affinity](https://microsoft.github.io/reverse-proxy/articles/session-affinity.html)

## Computer science

- [↑ Heap](<https://en.wikipedia.org/wiki/Heap_(data_structure)>), [\[2\]](https://en.wikipedia.org/wiki/Binary_heap)
- [↑ Binary Heap](https://www.geeksforgeeks.org/binary-heap)
- [↑ Why does a Binary Heap has to be a Complete Binary Tree?](https://stackoverflow.com/questions/25319305/why-does-a-binary-heap-has-to-be-a-complete-binary-tree)
- [↑ Top 10 Algorithms for the Coding Interview](https://www.youtube.com/watch?v=r1MXwyiGi_U)

## Web

- [↑ Ant Design](https://ant.design)
- [↑ Material Design](https://m3.material.io)
- <https://html5book.ru/html-html5>
- <https://html5book.ru/css-css3>
- [↑ Sololearn](https://apps.apple.com/us/app/sololearn-learn-to-code-apps/id1210079064)
- [↑ StyleDrop: Text-To-Image Generation in Any Style](https://styledrop.github.io)
- [↑ Muse: Text-To-Image Generation via Masked Generative Transformers](https://muse-model.github.io)

## Разобрать

- https://www.postgresql.org/docs/current/rules-materializedviews.html
- https://www.oreilly.com/library/view/microservices-antipatterns-and/9781492042716/ch01.html
- https://habr.com/ru/companies/ozontech/articles/708274/
- https://highload.ru/spb/2022/abstracts/9317
