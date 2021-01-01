# Documenting notes

## Computer science

* [Asymmetric cryptography](cs/asymmetric%20cryptography.md)
* [Generating self-signed certificate with .NET CLI](cs/generating%20certificate%20with%20dotnet%20cli.md)

## Design

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

## Programming tools

* [Ansible](programming/ansible/ansible.md)
* [Docker](programming/docker/docker.md)
  * [docker-compose](programming/docker/compose.md)
* [Elasticsearch](programming/elk/elastic.md)
* [Filebeat](programming/elk/filebeat.md)
* GCP
  * [Google Cloud Shell](programming/gcp/gcshell.md)
* [Git commands](programming/git/git.md)
* Kubernetes
  * [kubectl](programming/kubernetes/kubectl.md)
  * [Creating custom Kubernetes cluster using Ansible](programming/kubernetes/creating%20cluster.md)
  * [Flux](programming/flux.md)
  * [Kubernetes components](programming/kubernetes/components.md)
    * [Create a Kubernetes cluster](programming/kubernetes/create%20cluster.md)
    * [Deploy an app](programming/kubernetes/deply%20app.md)
    * [Explore your app](programming/kubernetes/explore%20app.md)
    * [Expose your app publicly](programming/kubernetes/expose%20app.md)
    * [Scale your app](programming/kubernetes/scale.md)
    * [Update your app](programming/kubernetes/update.md)

## Unix

* [Environment variables](unix/environment%20variables.md)
* [macOS](unix/macos/macos.md)
  * [Finder](unix/macos/finder.md)
  * [Terminal](unix/macos/terminal.md)
* [Ubuntu](unix/ubuntu.md)
* [Nginx](unix/nginx.md)
* [shell](unix/shell.md)
  * [Bourne again shell (bash)](unix/bash.md)
* Tools
  * [apt](unix/apt.md)
  * [chmod](unix/chmod.md)
  * [chown](unix/chown.md)
  * [crontab](unix/crontab.md)
  * [df](unix/df.md)
  * [du](unix/du.md)
  * [htop](unix/htop.md)
  * [iproute2](unix/iproute2.md)
  * [less](unix/less.md)
  * [rg](unix/rg.md)
  * [rsync](unix/rsync.md)
  * [scp](unix/scp.md)
  * [ssh](unix/ssh.md)
  * [systemctl](unix/systemctl.md)
  * [tail](unix/tail.md)
  * [tar](unix/tar.md)
  * [tmux](unix/tmux.md)
  * [vim](unix/vim.md)
* [User management](unix/user%20managment.md)

## C#

* [Attributes](/csharp/attributes.md)
  * [Nullable static analysis attributes](/csharp/nullable%20static%20analysis%20attributes.md)
* Concurrency
  * [Async/await best practices](/csharp/asynchronous/async%20await%20best%20practices.md)
  * [Async lambdas](csharp/asynchronous/async%20lambdas.md)
  * [Awaiting multiple tasks](csharp/asynchronous/awaiting%20multiple%20tasks.md)
  * [Exceptions in tasks](/csharp/asynchronous/exceptions.md)
  * [Execution context](/csharp/asynchronous/execution%20context.md)
  * [Reporting task progress](/csharp/asynchronous/reporting%20progress.md)
  * [Task scheduler](/csharp/asynchronous/task%20scheduler.md)
* [Constructor execution order](/csharp/constructor%20execution%20order.md)
* [Covariance and contravariance](/csharp/covariance%20and%20contravariance.md)
* [Equality operator](/csharp/equality%20operator.md)
* [Extension methods](csharp/extension%20methods.md)
* [Keywords](csharp/keywords/keywords.md)
  * [yield](csharp/keywords/yield.md)
* [Lambda expressions](/csharp/lambda%20expressions.md)
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

## Miscellaneous

* ASP.NET
  * [Host](asp.net/host.md)
* Databases
  * [PostgreSQL](db/postgres/postgres.md)
  * [PL/SQL](db/oracle/plsql.md)
* Entity Framework Core
  * [Installation](efcore/installation.md)
  * [Commands](efcore/commands.md)
* [Regular expressions](programming/regular%20expressions/regular%20expressions.md)
* [Unit testing best practices](programming/testing/unit%20testing%20best%20practices.md)

## Web

* [CSS](web/css/css.md)
* [gRPC](web/grpc.md)
* [HTML](web/html/html.md)
* [HTTP](web/http/http.md)
* [JavaScript](web/javascript/javascript.md)
* [React](web/react/react.md)
* [REST](web/rest.md)
* Security
  * [↑ Cross-site request forgery (XSRF or CSRF)](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  * [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)
  * [OAuth 2.0](web/oauth.md)
* [Web APIs](web/api/api.md)
* [WebSocket](websocket.md)