# Technical documentation

[Knowledge map (SVG)](https://raw.githubusercontent.com/mialkin/documentation/master/knowledge%20map.svg)

## Table of cоntents

- [.NET](#dotnet)
- [Computer science](#computer-science)
- [Databases](#databases)
- [Miscellaneous](#miscellaneous)
- [Networking](#networking)
- [Programming](#programming)
- [Software design](#software-design)
- [Unix](#unix)
- [Web](#web)

<a name="dotnet"></a>

## .NET

- C#
  - Boxing and unboxing
  - [Access modifiers](dotnet/csharp/access%20modifiers.md)
  - [Constructor execution order](dotnet/csharp/constructor%20execution%20order.md)
  - [Covariance and contravariance](dotnet/csharp/covariance%20and%20contravariance.md)
  - [Nullable reference types](dotnet/csharp/nullable%20reference%20types.md)
  - Operators and expressions
    - [Equality operator](dotnet/csharp/equality%20operator.md)
- .NET API
  - System
    - Classes
      - Array
      - Delegate
        - Action\<T>
        - Func\<TResult>
      - Exception
      - GC
        - Properties
          - MaxGeneration
        - Methods
          - Collect(Int32)
          - CollectionCount(Int32)
          - GetGeneration(Object)
          - GetTotalMemeory(Boolean)
          - SuppressFinalize(Object)
          - WaitForPendingFinalizers()
      - Lazy\<T>
      - MarshalByRefObject
      - Object
        - Methods
          - [Equals](dotnet/api/system/object/equals.md)
          - [GetType](dotnet/api/system//object/getType.md)
          - [ToString](dotnet/api/system/object/toString.md)
          - [Finalize](dotnet/api/system/object/finalize.md)
          - [MemberwiseClone](dotnet/api/system/object/memberwiseClone.md)
          - [ReferenceEquals](dotnet/api/system/object/referenceEquals.md)
      - String
      - Tuple\<T1>
      - ValueType
        - Enum
        - Guid
        - Nullable\<T>
    - Enums
    - Interfaces
      - IComparable\<T>
      - ICloneable
      - IDisposable
    - Structs
  - System.Collections.Concurrent
    - ConcurrentDictionary\<T>
    - ConcurrentQueue\<T>
    - ConcurrentStack\<T>
  - System.IO
    - FileStream
    - MemoryStream
  - System.Linq
    - Interfaces
      - [IQueryable\<T>](dotnet/api/system/linq/iqueryable.md)
  - System.Threading
    - [Task Parallel Library (TPL)](dotnet/api/system/threading/tpl.md)
    - Classes
      - Interlocked
      - Monitor
      - Mutex
      - ReaderWriterLock
      - ReaderWriterSlimLock
      - Semaphore
      - SynchronizationContext
      - [Thread](dotnet/api/system/threading/thread.md)
      - ThreadPool
    - Structs
      - CancellationToken
      - SpinLock
      - SpinWait
  - System.Threading.Tasks
    - Classes
      - Parallel
      - [Task & Task\<T>](dotnet/api/system/threading/tasks/task.md)
      - TaskFactory
- Common Language Infrastructure (CLI)
  - Common Intermediate Language (CIL)
  - CLI implementations
    - .NET Standard
      - .NET Core
        - CoreCLR
          - Memory managment
            - Garbage collector (GC)
            - Managed heap
              - Small object heap (SOH)
          - Just-in-time (JIT) compilation
        - CoreFx
          - ASP.NET Core
            - Dependency injection
            - Middleware
            - Host
            - Configuration
            - Options
            - Environments
            - Logging
            - Routing
            - Security
              - Authentication
              - Authorization
            - [↑ SignalR](https://dotnet.microsoft.com/apps/aspnet/signalr)

## Computer science

- Data structures
  - Hash-based structures
    - Hash table
    - Hash tree
- Algorithms
  - Sorting algorithms
    - Insertion sort
    - Selection sort
    - Merge sort
    - Quick sort
    - Bubble sort
  - Analysis of algorithm
    - Time complexity
    - Space complexity
    - Big O notation
- Object-oriented programming (OOP)
  - Abstraction
  - Incapsulation
  - Inheritance
  - Polymorphism
- Cryptography
  - Symmetric cryptography
  - [Asymmetric cryptography](cs/asymmetric%20cryptography.md)

## Databases

- ACID
  - Atomicity
  - Consistency
  - Isolation
  - Durability
- Transaction
  - Isolation levels
    - Read uncommitted
    - Read commmitted
    - Repeatable read
    - Serializable
- Execution plan
- Trigger
- Stored procedure
- Function
- Cursor
- SQL
  - T-SQL
  - PL/SQL
- Document databases
  - CouchDB
  - Elasticsearch
  - MongoDB
- Relational databases
  - Microsoft SQL Server
  - Oracle Database
  - SQLite
  - PostgreSQL
- In-memory databases
  - Memcached
  - Redis

## Miscellaneous

- [Reading list](reading%20list.md)
- [Study list](study%20list.md)
- [Перевод технических терминов](translation.md)

## Networking

- HTTP
  - HTTP/2
    - [gRPC & Protobuf](network/grpc.md)
- WebSocket

## Programming

- Git
  - Git flows
    - [Ветки](programming/git/branches.md)
      - [Релиз](programming/git/release.md)
      - [Именование релизных веток](programming/git/branch%20names.md)
- [Regular expressions](programming/regular%20expressions/regular%20expressions.md)
- Testing
  - Unit testing
    - Unit testing libraries
      - NUnit
    - Mock objects
  - Integration testing
  - Load testing
  - Stress testing
- Tools
  - [Docker](programming/docker/docker.md)
  - [Kubernetes](programming/kubernetes/kubernetes.md)
  - GitLab

## Software design

- Design principles
  - SOLID
    - Single responsibility principle
    - Open-closed principle
    - Liskov substitution principle
    - Interface segregation principle
    - [Dependency Inversion Principle](software%20design/dependency%20inversion/dependency%20inversion.md)
  - DRY
  - KISS
  - YAGNI
  - [Composition over inheritance](software%20design/composition%20over%20inheritance.md)
- Unified Modeling Language (UML)
  - Diagrams
    - Class diagram
    - Sequence diagram
- [Design patterns](software%20design/design%20patterns/design%20patterns.md)

## Unix

- macOS
- Linux
  - CentOS
    - [YUM](unix/yum.md)
  - [Ubuntu](unix/ubuntu.md)
    - [APT](unix/apt.md)
- [shell](unix/shell.md)
  - [Bourne again shell (bash)](unix/bash.md)
  - Commands
    - System
      - [crontab](unix/crontab.md)
      - [df](unix/df.md)
      - [du](unix/du.md)
      - [htop](unix/htop.md)
      - [systemctl](unix/systemctl.md)
      - [tmux](unix/tmux.md)
    - Files
      - [less](unix/less.md)
      - [rg](unix/rg.md)
      - [tail](unix/tail.md)
      - [tar](unix/tar.md)
      - [vim](unix/vim.md)
    - Network
      - [rsync](unix/rsync.md)
      - [scp](unix/scp.md)
      - [ssh](unix/ssh.md)
- [Nginx](unix/nginx.md)

## Web

- HTML
- CSS
- JavaScript
- Web services
  - Representational State Transfer (REST)
    - JSON Web Token (JWT)
    - OpenAPI
    - [OAuth 2.0](web/oauth.md)
