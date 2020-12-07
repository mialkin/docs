# Technical documentation

## ASP.NET

* Authentication
* Authorization
* [↑ Caching](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
* [↑ Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration)
* [↑ Dependency injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection)
* [↑ Environments](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments)
* [↑ Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters)
* [Host](asp.net/host.md)
* [↑ Hosted service](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/hosted-services)
* [Logging](asp.net/logging.md)
* [Middleware](asp.net/middleware.md)
* [↑ Model Binding](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/model-binding)
* [Model validation](asp.net/model%20validation.md)
* [↑ Options](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options)
* [↑ Performance](https://docs.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
* [↑ Routing](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing)
* Security
* [↑ Servers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers)
* [SignalR](asp.net/signalr.md)
* [↑ Upload files](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/file-uploads)

## C#

* Boxing and unboxing
* [Access modifiers](/csharp/access%20modifiers.md)
* [Asynchronous programming](/csharp/asynchronous%20programming.md)
  * [Async/await best practices](/csharp/asynchronous/async%20await%20best%20practices.md)
  * [Async lambdas](csharp/asynchronous/async%20lambdas.md)
  * [Exceptions in Tasks](/csharp/asynchronous/exceptions.md)
  * [Execution Context](/csharp/asynchronous/execution%20context.md)
  * [Task scheduler](/csharp/asynchronous/task%20scheduler.md)
* [Attributes](/csharp/attributes.md)
  * [Nullable static analysis attributes](/csharp/nullable%20static%20analysis%20attributes.md)
* [Constructor execution order](/csharp/constructor%20execution%20order.md)
* [Covariance and contravariance](/csharp/covariance%20and%20contravariance.md)
* CLR
* [Extension methods](csharp/extension%20methods.md)
* Just-in-time (JIT) compilation
* [Keywords](csharp/keywords/keywords.md)
  * Contextual keywords
    * [yield](csharp/keywords/yield.md)
* [Lambda expressions](/csharp/lambda%20expressions.md)
* [Nullable reference types](/csharp/nullable%20reference%20types.md)
* Memory managment
  * [↑ Garbage collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection)
  * Managed heap
    * Small object heap (SOH)
* Operators and expressions
  * [Equality operator](/csharp/equality%20operator.md)
* [Reflection](/csharp/reflection.md)
* [Task Parallel Library (TPL)](csharp/tpl.md)
  * [↑ Task-based asynchronous pattern](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
  * [Reporting task progress](/csharp/asynchronous/reporting%20progress.md)
* [Tricks](/csharp/tricks.md)
* [↑ Document your code with XML comments](https://docs.microsoft.com/en-us/dotnet/csharp/codedoc)
* [↑ Local functions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions)

## EF Core

[EF Core](efcore/efcore.md)

## .NET API

* System
  * Classes
    * Array
    * [Delegate](dotnet/system/delegate/delegate.md)
      * Action\<T>
      * Func\<TResult>
      * [Event](dotnet/system/delegate/event.md)
    * Exception
    * GC
      * Properties
        * MaxGeneration
      * Methods
        * Collect(Int32)
        * CollectionCount(Int32)
        * GetGeneration(Object)
        * GetTotalMemory(Boolean)
        * SuppressFinalize(Object)
        * WaitForPendingFinalizers()
    * Lazy\<T>
    * MarshalByRefObject
    * Object
      * Methods
        * [Equals](dotnet/system/object/equals.md)
        * [GetType](dotnet/system//object/getType.md)
        * [ToString](dotnet/system/object/toString.md)
        * [Finalize](dotnet/system/object/finalize.md)
        * [MemberwiseClone](dotnet/system/object/memberwiseClone.md)
        * [ReferenceEquals](dotnet/system/object/referenceEquals.md)
    * String
    * Tuple\<T1>
    * ValueType
      * Enum
      * Guid
      * Nullable\<T>
  * Enums
  * Interfaces
    * IComparable\<T>
    * ICloneable
    * IDisposable
  * Structs
* System.Collections.Concurrent
  * ConcurrentDictionary\<T>
  * ConcurrentQueue\<T>
  * ConcurrentStack\<T>
* System.Collections.Generic
  * Classes
    * [Dictionary](dotnet/system/collections/generic/dictionary.md)
  * Interfaces
    * [IAsyncEnumerable\<T>](dotnet/system/collections/generic/iasyncenumerable.md)
    * [IEnumerable](dotnet/system/collections/generic/ienumerable.md)
* System.IO
  * FileStream
  * MemoryStream
* System.Linq
  * [Expression tree](dotnet/system/linq/expression%20tree.md)
  * Interfaces
    * [IQueryable\<T>](dotnet/system/linq/iqueryable.md)
* System.Threading
  * Classes
    * Interlocked
    * Monitor
    * Mutex
    * ReaderWriterLock
    * ReaderWriterLockSlim
    * [Semaphore](dotnet/system/threading/semaphore.md)
    * [SemaphoreSlim](dotnet/system/threading/semaphoreslim.md)
    * [↑ Synchronization context](https://docs.microsoft.com/en-us/dotnet/api/system.threading.synchronizationcontext)
    * [Thread](dotnet/system/threading/thread.md)
    * [ThreadPool](dotnet/system/threading/threadpool.md)
  * Structs
    * CancellationToken
    * SpinLock
    * SpinWait
* System.Threading.Tasks
  * Classes
    * Parallel
    * [Task](dotnet/system/threading/tasks/task/task.md)
      * [Properties](dotnet/system/threading/tasks/task/properties.md)
    * [Task\<TResult>](dotnet/system/threading/tasks/task_t/task.md)
    * TaskFactory
  * Structs
    * [ValueTask\<TResult>](dotnet/system/threading/tasks/valuetask_t.md)

## Computer science

* Algorithms
  * Sorting algorithms
    * Insertion sort
    * Selection sort
    * Merge sort
    * Quick sort
    * Bubble sort
  * Analysis of algorithm
    * Time complexity
    * Space complexity
    * Big O notation
* [Asymmetric cryptography](cs/asymmetric%20cryptography.md)
* Data structures
  * Hash-based structures
    * Hash table
    * Hash tree
* [Object-oriented programming (OOP)](cs/oop.md)

## Databases

* ACID
  * Atomicity
  * Consistency
  * Isolation
  * Durability
* Cursor
* Database normalization
* Document databases
  * CouchDB
  * Elasticsearch
  * MongoDB
* Execution plan
* Function
* In-memory databases
  * Redis
* Relational databases
  * Microsoft SQL Server
  * Oracle Database
  * [SQLite](db/sqlite.md)
  * [PostgreSQL](db/postgres/postgres.md)
* Stored procedure
* SQL
  * T-SQL
  * [PL/SQL](db/oracle/plsql.md)
* [Terminology](db/terminology.md)
* Transaction
  * Isolation levels
    * Read uncommitted
    * Read commmitted
    * Repeatable read
    * Serializable
* Trigger
* Window function

## Design

* Architectural patterns
  * [↑ API gateway](https://www.nginx.com/blog/building-microservices-using-an-api-gateway)
  * CQRS
  * Event sourcing
  * [↑ MVC](https://docs.microsoft.com/en-us/aspnet/core/mvc/overview?)
  * Repository
  * Service locator
  * Unit of work
* [Design patterns](design/design%20patterns/design%20patterns.md)
  * Creational
    * [Abstract Factory](design/design%20patterns/abstract%20factory.md)
    * [Builder](design/design%20patterns/builder.md)
    * [Factory Method](design/design%20patterns/factory%20method.md)
    * [Prototype](design/design%20patterns/prototype.md)
    * [Singleton](design/design%20patterns/singleton.md)
  * Structural
    * [Adapter](design/design%20patterns/adapter.md)
    * [Bridge](design/design%20patterns/bridge.md)
    * [Composite](design/design%20patterns/composite.md)
    * [Decorator](design/design%20patterns/decorator.md)
    * [Facade](design/design%20patterns/facade.md)
    * [Flyweight](design/design%20patterns/flyweight.md)
    * [Proxy](design/design%20patterns/proxy.md)
  * Behavioral
    * [Chain of Responsibility](design/design%20patterns/chain%20of%20responsibility.md)
    * [Command](design/design%20patterns/command.md)
    * Interpreter
    * [Iterator](design/design%20patterns/iterator.md)
    * [Mediator](design/design%20patterns/mediator.md)
    * [Memento](design/design%20patterns/memento.md)
    * [Observer](design/design%20patterns/observer.md)
    * [State](design/design%20patterns/state.md)
    * [Strategy](design/design%20patterns/strategy.md)
    * [Template Method](design/design%20patterns/template%20method.md)
    * [Visitor](design/design%20patterns/visitor.md)
* Design principles
  * [Composition over inheritance](design/composition%20over%20inheritance.md)
  * [DRY](design/dry.md)
  * [Explicit Dependencies Principle](design/explicit%20dependencies%20principle.md)
  * [Idempotency](design/idempotency.md)
  * [KISS](design/kiss.md)
  * [SOLID](design/solid/solid.md)
    * [Single responsibility principle](design/solid/srp.md)
    * Open-closed principle
    * Liskov substitution principle
    * Interface segregation principle
    * [Dependency Inversion Principle](design/solid/dip/dip.md)
  * [YAGNI](design/yagni.md)
* [Unified Modeling Language (UML)](design/uml.md)

### Design links

* [↑ Anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern)
* [↑ Java Desing Principles](https://java-design-patterns.com/principles/)

## Programming

* [Ansible](programming/ansible/ansible.md)
* [Docker](programming/docker/docker.md)
  * [Docker Compose](programming/docker/compose.md)
* [Git](programming/git/git.md)
  * Git flows
    * [Ветки](programming/git/branches.md)
      * [Релиз](programming/git/release.md)
      * [Именование релизных веток](programming/git/branch%20names.md)
* [Kubernetes](programming/kubernetes/kubernetes.md)
  * [Creating custom Kubernetes cluster using Ansible](programming/kubernetes/creating%20cluster.md)
  * [Flux](programming/flux.md)
  * [Kubernetes components](programming/kubernetes/components.md)
  * [Minikube](programming/kubernetes/minikube.md)
    * Learning basics
      * [Create a Kubernetes cluster](programming/kubernetes/create%20cluster.md)
      * [Deploy an app](programming/kubernetes/deply%20app.md)
      * [Explore your app](programming/kubernetes/explore%20app.md)
      * [Expose your app publicly](programming/kubernetes/expose%20app.md)
      * [Scale your app](programming/kubernetes/scale.md)
      * [Update your app](programming/kubernetes/update.md)
* [↑ GitOps](programming/git/gitops.md)
* [Microservices](programming/microservices.md)
* [Regular expressions](programming/regular%20expressions/regular%20expressions.md)
* [Telegram bot](programming/telegram/telegram%20bot.md)
* Testing
  * Unit testing
    * [Unit testing best practices](programming/testing/unit%20testing%20best%20practices.md)
  * Integration testing
  * Load testing
  * [↑ Moq4 quickstart](https://github.com/Moq/moq4/wiki/Quickstart)
  * [↑ Selenium](https://www.selenium.dev)
  * Stress testing

## Unix

* [Environment variables](unix/environment%20variables.md)
* [macOS](unix/macos/macos.md)
  * [Finder](unix/macos/finder.md)
  * [Jekyll](unix/macos/jekyll.md)
  * [Terminal](unix/macos/terminal.md)
* Linux
  * CentOS
    * [YUM](unix/yum.md)
  * [Ubuntu](unix/ubuntu.md)
    * [APT](unix/apt.md)
* [Nginx](unix/nginx.md)
* [shell](unix/shell.md)
  * [Bourne again shell (bash)](unix/bash.md)
  * Tools
    * System
      * [crontab](unix/crontab.md)
      * [df](unix/df.md)
      * [du](unix/du.md)
      * [htop](unix/htop.md)
      * [systemctl](unix/systemctl.md)
      * [tmux](unix/tmux.md)
    * Files
      * [chmod](unix/chmod.md)
      * [chown](unix/chown.md)
      * [less](unix/less.md)
      * [rg](unix/rg.md)
      * [tail](unix/tail.md)
      * [tar](unix/tar.md)
      * [vim](unix/vim.md)
    * Network
      * [iproute2](unix/iproute2.md)
      * [rsync](unix/rsync.md)
      * [scp](unix/scp.md)
      * [ssh](unix/ssh.md)
* [User management](unix/user%20managment.md)

## Web

* [CSS](web/css/css.md)
* [CORS](web/cors.md)
* [gRPC](web/grpc.md)
* [HTML](web/html/html.md)
* [↑ HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
  * [HTTP headers](web/http%20headers.md)
    * [↑ Accept](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept)
    * [↑ Accept-Encoding](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding)
    * [↑ Strict-Transport-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security)
  * [↑ HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
    * [↑ 401 Unauthorized](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401)
    * [↑ 405 Method Not Allowed](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/405)
    * [↑ 429 Too Many Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)
* [JavaScript](web/javascript/javascript.md)
* [JSON Web Token (JWT)](web/jwt.md)
* [OAuth 2.0](web/oauth.md)
* [↑ OpenAPI 3.0](https://swagger.io/blog/news/whats-new-in-openapi-3-0)
* [React](web/react/react.md)
* [REST](web/rest.md)
* Security
  * [↑ Cross-site request forgery (XSRF or CSRF)](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  * [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)
* [Web APIs](web/api/api.md)
* [WebSocket](websocket.md)
