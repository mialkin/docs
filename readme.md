# Documenting notes

## Tools

* [Ansible](tools/ansible/ansible.md)
* [Docker](tools/docker/docker.md)
  * [docker-compose](tools/docker/compose.md)
* [Git](tools/git/git.md)
* Kubernetes
  * [Flux](tools/kubernetes/flux.md)
  * [kubectl](tools/kubernetes/kubectl.md)
  * [Creating custom Kubernetes cluster using Ansible](tools/kubernetes/creating%20cluster.md)
* [Redis](tools/redis.md)

## Unix

* [Environment variables](unix/environment%20variables.md)
* [macOS](unix/macos/macos.md)
  * [Hotkeys](unix/macos/hotkeys.md)
  * [Terminal](unix/macos/terminal.md)
* [Ubuntu](unix/ubuntu.md)
* [Nginx](unix/nginx.md)
* [shell](unix/shell.md)
  * [bash](unix/bash.md)
* [Tools](unix/tools.md)
  * [crontab](unix/crontab.md)
  * [ssh](unix/ssh.md)
  * [tmux](unix/tmux.md)
  * [vim](unix/vim.md)
* [User management](unix/user%20managment.md)

## Web

* [CSS](web/css/css.md)
* [GraphQL](web/graphql.md)
* [gRPC](web/grpc.md)
* [HTML](web/html/html.md)
* [HTTP](web/http/http.md)
* [JavaScript](web/javascript/javascript.md)
* [REST](web/rest.md)
* [OAuth](web/oauth.md)
* [OpenID Connect](web/oidc.md)
* Security
  * [↑ Cross-site request forgery (XSRF or CSRF)](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  * [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)
* [Web APIs](web/api/api.md)

## Miscellaneous

* ASP.NET
  * [Host](asp.net/host.md)
* Databases
  * [ACID](db/transactions/acid.md)
  * [Isolation levels](db/transactions/isolation%20levels.md)
  * [PostgreSQL](db/postgres/postgres.md)
  * [PL/SQL](db/oracle/plsql.md)
* EF Core
  * [EF Core tools](efcore/tools.md)
* [Regular expressions](tools/regular%20expressions/regular%20expressions.md)
* [Unit testing best practices](tools/testing/unit%20testing%20best%20practices.md)

## C#

* [Attributes](/csharp/attributes.md)
  * [Nullable static analysis attributes](/csharp/nullable%20static%20analysis%20attributes.md)
* Asynchronous programming (old)
  * [Async/await best practices](/csharp/asynchronous/async%20await%20best%20practices.md)
  * [Async lambdas](csharp/asynchronous/async%20lambdas.md)
  * [Awaiting multiple tasks](csharp/asynchronous/awaiting%20multiple%20tasks.md)
  * [Exceptions in tasks](/csharp/asynchronous/exceptions.md)
  * [Execution context](/csharp/asynchronous/execution%20context.md)
  * [Reporting task progress](/csharp/asynchronous/reporting%20progress.md)
  * [Task scheduler](/csharp/asynchronous/task%20scheduler.md)
* [Concurrency](csharp/concurrency/concurrency.md)
  * [Asynchronous programming](csharp/concurrency/asynchronous%20programming.md)
    * [Returning from task](csharp/concurrency/returning%20from%20task.md)
    * System.Collections.Generic
      * Interfaces
        * [IAsyncEnumerable\<T>](dotnet/system/collections/generic/iasyncenumerable.md)
  * [Parallel programming](csharp/concurrency/parallel%20programming.md)
  * [Reactive programming](csharp/concurrency/reactive%20programming.md)
  * [Dataflow programming](csharp/concurrency/dataflow%20programming.md)
  * [Collections](csharp/concurrency/collections.md)
    * System.Collections.Concurrent
      * ConcurrentDictionary\<T>
      * ConcurrentQueue\<T>
      * ConcurrentStack\<T>
  * [Synchronization](csharp/concurrency/synchronization.md)
* [Constructor execution order](/csharp/constructor%20execution%20order.md)
* [Covariance and contravariance](/csharp/covariance%20and%20contravariance.md)
* [Equality operator](/csharp/equality%20operator.md)
* [Extension methods](csharp/extension%20methods.md)
* [Keywords](csharp/keywords/keywords.md)
  * [yield](csharp/keywords/yield.md)
* [Lambda expressions](/csharp/lambda%20expressions.md)
* [Memory managment](csharp/memory%20managment.md)
* [Nullable reference types](/csharp/nullable%20reference%20types.md)
* [Ref vs out](csharp/ref%20vs%20out.md)
  
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
* System.Collections.Generic
  * Classes
    * [Dictionary](dotnet/system/collections/generic/dictionary.md)
  * Interfaces
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

* [Asymmetric cryptography](cs/asymmetric%20cryptography.md)
* [Advanced Encryption Standard (AES)](cs/aes.md)
* [Generating self-signed certificate](cs/generating%20certificate.md)
* [TLS handshake](cs/tls%20handshake.md)

## Software design

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
  * [Separation of concerns](design/separation%20of%20concerns.md)
  * [SOLID](design/solid/solid.md)
    * [Single responsibility principle](design/solid/srp.md)
    * Open-closed principle
    * Liskov substitution principle
    * Interface segregation principle
    * [Dependency Inversion Principle](design/solid/dip/dip.md)
  * [YAGNI](design/yagni.md)