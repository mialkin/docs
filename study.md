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
  - [Dumb questions](#dumb-questions)

## Tools

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
- [↑ Qodana](https://www.jetbrains.com/qodana).
- [↑ Apache Airflow](https://airflow.apache.org)
- [↑ .NET Aspire](https://blog.jetbrains.com/dotnet/2024/02/19/jetbrains-rider-and-the-net-aspire-plugin/)

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
- [↑ EF Core Change Tracking](https://learn.microsoft.com/en-us/ef/core/change-tracking/).
- [↑ EF Core Interceptors](https://learn.microsoft.com/en-us/ef/core/logging-events-diagnostics/interceptors)

## Software architecture and design

- [↑ Architectural Katas](https://nealford.com/katas)
- DDD
- Architectural patterns
  - [↑ EventSourcing.NetCore](https://github.com/oskardudycz/EventSourcing.NetCore)
  - Microservices related patterns
    - [↑ Ambassador](https://docs.microsoft.com/en-us/azure/architecture/patterns/ambassador)
    - [↑ Backends for Frontends](https://microservices.io/patterns/apigateway.html)
    - Retry
      - [↑ Implement HTTP call retries with exponential backoff with IHttpClientFactory and Polly policies](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
- [↑ Почему авторизация сложно и причем здесь Занзибар?](https://www.youtube.com/watch?v=Tr5H8iG0FzI)
  - <https://zanzibar.academy>
- [↑ Cloud Desing Patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/)
- [↑ Distributed Systems lecture series](https://www.youtube.com/playlist?list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB)
- [↑ AsyncAPI & event-driven architecture](https://www.asyncapi.com/)
- [↑ Lock-free коллекции в .NET 6](https://www.youtube.com/watch?v=-fTrew8atpk)
  - [↑ Посмотреть доклады по .NET на канале IT и т.д.](https://www.youtube.com/@IT_i_td/videos)
- [↑ Текст. Одна платформа, чтобы править всеми](https://habr.com/ru/companies/ozontech/articles/708274/)
  - [↑ Видео. Одна платформа, чтобы править всеми](https://highload.ru/spb/2022/abstracts/9317)
- [↑ Канал "Hard&Soft Skills"](https://www.youtube.com/@hardsoftskills9958)

## Programming

- [↑ Orleans](https://learn.microsoft.com/en-us/dotnet/orleans/overview)
- Blue-green deployment
- [↑ "Repository и UnitOfWork в 2020 году, must have или антипаттерн?"](https://www.youtube.com/watch?v=3yPpL1rEK9o)
- [↑ Domain-Driven Refactoring - Jimmy Bogard - NDC London 2022](https://www.youtube.com/watch?v=f64tZ90Dntg)
- [↑ Тестируем микросервисы правильно](https://www.youtube.com/watch?v=yj3sndjLHEA)
- [↑ Чистая архитектура на практике](https://www.youtube.com/watch?v=Bd83nPK_K3U)
- [↑ Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)
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
  - [↑ Оптимизация сборки мусора в высоконагруженном .NET сервисе](https://habr.com/ru/articles/452298/)
  - [↑ CLRium #5: Курс "Garbage Collector"](https://www.youtube.com/watch?v=DVnmGW6964o)
  - [↑ Memory leaks](https://github.com/sebastienros/memoryleak)
  - Learn dotTrace
  - `WaitForPendingFinalizers`
  - [↑ When should I use `GC.SuppressFinalize()`?](https://stackoverflow.com/questions/151051/when-should-i-use-gc-suppressfinalize)
  - [↑ Finalizers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/finalizers)
    - [↑ Cleaning up unmanaged resources](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/unmanaged)
    - [↑ Throwing exception in finalizer to enforce Dispose calls:](https://stackoverflow.com/questions/20358401/throwing-exception-in-finalizer-to-enforce-dispose-calls)
- [↑ dotnet-monitor](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/dotnet-monitor)
- [↑ Pact NET. Consumer driven contract testing](https://github.com/pact-foundation/pact-net)
- [↑ Allure framework](https://docs.qameta.io/allure)
  - [↑ Allure.XUnit](https://github.com/Tinkoff/Allure.XUnit)
- Data masking
  - [↑ Destructurama.Attributed](https://github.com/destructurama/attributed)
  - [↑ Serilog.Enrichers.Sensitive](https://github.com/serilog-contrib/Serilog.Enrichers.Sensitive/tree/master)
  - [↑ What is data masking?](https://aws.amazon.com/what-is/data-masking/)
- [↑ NSubstitute](https://youtu.be/1oeMTz7LwrU?si=HOMpYy5AtUpSaPzY&t=421)

## C&#35;

- [↑ Access modifiers](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/access-modifiers)
- [↑ Expression trees](https://tyrrrz.me/blog/expression-trees)
  - [↑ Expression Trees in C#](https://code-maze.com/csharp-expression-trees)
  - [↑ How to Build Dynamic Queries With Expression Trees in C#](https://code-maze.com/dynamic-queries-expression-trees-csharp/)
  - Generate code from created expression tree for some mapper. Use generated code to map 2 objects
- IQueryable interface
- `IDisposable`
  - [↑ Struct and IDisposable](https://stackoverflow.com/questions/7914423/struct-and-idisposable)
  - [↑ To box or not to box](https://ericlippert.com/2011/03/14/to-box-or-not-to-box/)
  - [↑ Implement a Dispose method](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose)
  - [↑ Implement a DisposeAsync method](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-disposeasync)
  - [↑ Disposable pattern](https://medium.com/@mypascal2000/disposable-patterns-ffa2145619e2)
- [↑ Writing async/await from scratch in C# with Stephen Toub](https://www.youtube.com/watch?v=R-z2Hv-7nxk).
- [↑ How Async/Await Really Works in C#](https://devblogs.microsoft.com/dotnet/how-async-await-really-works/)
- [↑ Smallest .Net ref type is 12 bytes (or why you should consider using value types)](https://theburningmonk.com/2015/07/smallest-net-ref-type-is-12-bytes-or-why-you-should-consider-using-value-types/)
- [↑ Weak reference](https://docs.microsoft.com/en-us/dotnet/api/system.weakreference?view=netcore-3.1)
- [↑ Clean Up Your Client to Business Logic Relationship With a Result Pattern (C#)](https://alexdunn.org/2019/02/25/clean-up-your-client-to-business-logic-relationship-with-a-result-pattern-c/)
- [↑ Asynchronous TCP/IP, Part 1](https://www.youtube.com/watch?v=RJOdB5ly7jk)
- [↑ Async/await и механизм реализации в C# 5.0](https://habr.com/ru/articles/260217)
- [↑ Compile code programmatically by using C# compiler](https://learn.microsoft.com/en-us/troubleshoot/developer/visualstudio/csharp/language-compilers/compile-code-using-compiler)
- Just-In-Time IL code generation
- Try use [↑ ILSpy built with Avalonia](https://github.com/icsharpcode/AvaloniaILSpy)
- [↑ Reflection and Reflection.Emit in C#](https://www.c-sharpcorner.com/UploadFile/puranindia/reflection-and-reflection-emit-in-C-Sharp)
- [↑ Пишем код, когда пишем код: сорс-генераторы](https://habr.com/ru/companies/tinkoff/articles/766916/)
- [↑ CLR via C#](https://www.amazon.com/CLR-via-4th-Developer-Reference/dp/0735667454/) or [↑ The Book of the Runtime](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/README.md)
- Почему у структуры нет индекса синхронизации? [↑ Внутреннее устройство ссылочных типов C#](https://professorweb.ru/my/csharp/optimization/level2/2_1.php). Джеффри Рихтер
- Как устроен `Task` и `ValueTask`
- Monads
  - [↑ Monads explained in C#](https://mikhail.io/2016/01/monads-explained-in-csharp/)
  - [↑ The Maybe Monad (C#)](https://www.dotnetcurry.com/patterns-practices/1510/maybe-monad-csharp)
  - [↑ "Maybe" monad through async/await in C# (No Tasks!)](https://habr.com/ru/articles/458692/)
- [↑ Autofac](https://github.com/autofac/Autofac)
- На странице interface в репозитории docs разобрать какие типы данных могут реализовывать интерфейсы
- Implement merge sort using TPL
- Выписать XoR, побитовый сдвиг влево/вправо и прочие операции в CS-секцию с примерами реализации на C#
- Что нужно чтобы использовать using и await (GetEnumerator, GetAwaiter)?
- Cohesion <https://www.youtube.com/watch?v=r1Ih_nV79eE>, <https://ru.wikipedia.org/wiki/Связность_(программирование)>
- Разобрать .`Cast<>`, как реализован?
- Почему разработчики языка разделили память на стек и кучу?
- Написать бинарное дерево на C#
- Написать стек, двусвязный и односвязный список на C#
- [↑ Async HTTP Server](https://jonlabelle.com/snippets/view/csharp/async-http-server-and-client)
- Process asynchronous tasks as they complete (C#)
  - <https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete>
  - <https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/>
  - <https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks>
- [↑ Concurrency Visualizer](https://docs.microsoft.com/en-us/visualstudio/profiling/concurrency-visualizer)
  - [↑ Debugging Lock Contention Performance Issues in C# .NET](https://michaelscodingspot.com/lock-contentions/)
- [↑ Adventure in learning Go for a C# dev! - Ken Faulkner](https://www.youtube.com/watch?v=0ghHBPNe6g4)
- [↑ MessagePack](https://github.com/neuecc/MessagePack-CSharp)
  - MessagePack vs Apache Avro
- EF Core
  - [↑ Eager, Lazy and Explicit Loading with Entity Framework Core](https://blog.jetbrains.com/dotnet/2023/09/21/eager-lazy-and-explicit-loading-with-entity-framework-core/)
  - Implement one-to-one, one-to-many, many-to-many in <https://github.com/mialkin/postgres>
- [↑ `SearchValues<T>`](https://www.youtube.com/watch?v=IzDMg916t98)
- [↑ Tutorial: Write your first analyzer and code fix](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/tutorials/how-to-write-csharp-analyzer-code-fix)
  - [↑ Menees.Analyzers](https://github.com/menees/Analyzers/tree/master)
- [↑ Inline и throw](https://habr.com/ru/articles/722374/)

## ASP.NET

- [↑ Combining ASP.NET Core validation attributes with Value Objects](https://enterprisecraftsmanship.com/posts/combining-asp-net-core-attributes-with-value-objects/)
- [↑ ASP.NET Core Best Practices](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/best-practices)
- [↑ FastEndpoints](https://fast-endpoints.com)
- [↑ Graceful shutdown](https://www.google.com/search?q=graceful+shutdown+asp.net+core&oq=Graceful+shutdown+asp&aqs=chrome.1.69i57j0i512j0i15i22i30j0i390i650l2.6447j0j7&sourceid=chrome&ie=UTF-8)
- [↑ Wolverine](https://wolverine.netlify.app/)

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

## Dumb questions

- `try { return 1; } finally { return 2; }` // Cannot jump out of the finally block
- [↑ Why there is no inheritance in static classes C#?](https://stackoverflow.com/a/774225/1833895)
- `i++ + ++i`
- [↑ Why C# structs do no support inheritance](https://pragmateek.com/why-c-structs-do-no-support-inheritance/)
- [↑ How are C# Generics implemented?](https://stackoverflow.com/questions/11436802/how-are-c-sharp-generics-implemented)

```csharp
Foo foo = 1;
Console.WriteLine(foo);

record Foo(int Value)
{
    public static implicit operator Foo(int value) => new(value + 1);

    public static Foo operator *(Foo right, Foo left) => right.Value * left.Value;

    public override string ToString() => (this * 1).Value.ToString();
}
// 5
```
