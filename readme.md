# Documenting notes

## TOC

- [Documenting notes](#documenting-notes)
  - [TOC](#toc)
  - [Tools](#tools)
  - [Unix](#unix)
  - [Databases](#databases)
  - [Software architecture and design](#software-architecture-and-design)
  - [Computer science](#computer-science)
  - [Web](#web)
  - [Networking](#networking)
  - [Security](#security)
  - [Python](#python)
  - [dotNET](#dotnet)
  - [C&#35;](#c)

## Tools

- [Ansible](tools/ansible/ansible.md)
- [Docker](tools/docker/docker.md)
  - [Dockerfile](tools/docker/dockerfiles.md)
  - [Docker Compose](tools/docker/docker%20compose.md)
- [Git](tools/git/git.md)
- Kubernetes
  - [Flux](tools/kubernetes/flux.md)
  - [gcloud](tools/kubernetes/gcloud.md)
  - [helm](tools/kubernetes/helm.md)
  - [kubectl](tools/kubernetes/kubectl.md)
  - [Creating custom Kubernetes cluster using Ansible](tools/kubernetes/creating%20cluster.md)
- [Redis](tools/redis.md)
- [Regular expressions](tools/regular%20expressions/regular%20expressions.md)
- Testing
  - [xUnit.net](tools/testing/xunit.md)
  - [Unit testing best practices](tools/testing/unit%20testing%20best%20practices.md)

## Unix

- [Environment variables](unix/environment%20variables.md)
- [macOS](unix/macos/macos.md)
  - [Hotkeys](unix/macos/hotkeys.md)
  - [Terminal](unix/macos/terminal.md)
- [Ubuntu](unix/ubuntu.md)
- [Nginx](unix/nginx.md)
- [Shell](unix/shell/shell.md)
  - [Shell scripting](unix/shell/shell%20scripting.md)
- [Tools](unix/tools.md)
  - [crontab](unix/crontab.md)
  - [ssh](unix/ssh.md)
  - [tmux](unix/tmux.md)
  - [vim](unix/vim.md)
- [User management](unix/user%20managment.md)

## Databases

- [ACID](db/transactions/acid.md)
- [Isolation levels](db/transactions/isolation%20levels.md)
- [PostgreSQL](db/postgres/postgres.md)
- [PL/SQL](db/oracle/plsql.md)
- [EF Core tools](db/efcore/efcoretools.md)

## Software architecture and design

- Architectural patterns
  - CQRS
  - Event Sourcing
  - MVC
  - [Repository ↑](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)
  - Microservices related patterns
    - Aggregator
    - [Ambassador ↑](https://docs.microsoft.com/en-us/azure/architecture/patterns/ambassador)
    - API Gateway
    - Circuit Breaker
    - [Backends for Frontends ↑](https://microservices.io/patterns/apigateway.html)
    - Retry
    - Service Layer
    - Service Locator
    - Sidecar
  - Unit of Work
- CAP theorem
- [Design patterns](design/design%20patterns/design%20patterns.md)
  - Creational
    - [Abstract Factory](design/design%20patterns/abstract%20factory.md)
    - [Builder](design/design%20patterns/builder.md)
    - [Factory Method](design/design%20patterns/factory%20method.md)
    - [Prototype](design/design%20patterns/prototype.md)
    - [Singleton](design/design%20patterns/singleton.md)
  - Structural
    - [Adapter](design/design%20patterns/adapter.md)
    - [Bridge](design/design%20patterns/bridge.md)
    - [Composite](design/design%20patterns/composite.md)
    - [Decorator](design/design%20patterns/decorator.md)
    - [Facade](design/design%20patterns/facade.md)
    - [Flyweight](design/design%20patterns/flyweight.md)
    - [Proxy](design/design%20patterns/proxy.md)
  - Behavioral
    - [Chain of Responsibility](design/design%20patterns/chain%20of%20responsibility.md)
    - [Command](design/design%20patterns/command.md)
    - Interpreter
    - [Iterator](design/design%20patterns/iterator.md)
    - [Mediator](design/design%20patterns/mediator.md)
    - [Memento](design/design%20patterns/memento.md)
    - [Observer](design/design%20patterns/observer.md)
    - [State](design/design%20patterns/state.md)
    - [Strategy](design/design%20patterns/strategy.md)
    - [Template Method](design/design%20patterns/template%20method.md)
    - [Visitor](design/design%20patterns/visitor.md)
- Design principles
  - [Composition over inheritance](design/composition%20over%20inheritance.md)
  - [DRY, KISS, separation of concerns, YAGNI](design/main%20principles.md)
  - [Explicit Dependencies Principle](design/explicit%20dependencies%20principle.md)
  - [SOLID](design/solid/solid.md)
    - [Single Responsibility Principle](design/solid/srp.md)
    - [Open-Closed Principle](design/solid/ocp.md)
    - [Liskov Substitution Principle](design/solid/lsp.md)
    - [Interface Segregation Principle](design/solid/isp.md)
    - [Dependency Inversion Principle](design/solid/dip/dip.md)

## Computer science

- [Arguments vs parameters](cs/argument%20vs%20parameter.md)
- First-class function
- Higher-order function
- [Idempotence](cs/idempotence.md)
- [OOP](cs/oop.md)
- Programming paradigms
  - Imperative
    - procedural
    - object-oriented
  - Declarative
    - functional
    - logic
    - mathematical
    - reactive
- [Sorting algorithms](cs/sorting%20algorithms.md)

## Web

- [CSS](web/css/css.md)
- [GraphQL](web/graphql.md)
- [gRPC](web/grpc.md)
- [HTML](web/html/html.md)
- [HTTP](web/http/http.md)
- [JavaScript](web/javascript/javascript.md)
- [REST](web/rest.md)
- [OAuth](web/oauth.md)
- [OpenID Connect](web/oidc.md)
- [Web APIs](web/api/api.md)

## Networking

- [Commands](networking/commands.md)
- [IP](networking/ip.md)
- [Ports](networking/ports.md)
- [Sockets](networking/sockets.md)
- [TCP](networking/tcp.md)
- [Wireshark](networking/wireshark/wireshark.md)

## Security

- [Asymmetric cryptography](security/asymmetric%20cryptography.md)
- [Advanced Encryption Standard (AES)](security/aes.md)
- [Generating self-signed certificate](security/generating%20certificate.md)
- [TLS handshake](security/tls%20handshake.md)
- Web
  - [↑ Cross-site request forgery (XSRF or CSRF)](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  - [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)

## Python

- [Python](python/python.md)
  - [Syntax](python/syntax.md)

## dotNET

- [.NET CLI](dotnet/cli.md)
- ASP&#46;NET
  - [Host](dotnet/asp.net/host.md)
- [PowerShell](dotnet/powershell.md)
  - [PowerShell scripting](dotnet/powershell%20scripting.md)

## C&#35;

- [.NET API browser ↑](https://docs.microsoft.com/en-us/dotnet/api)
- [.NET Framework 4.8 Reference Source ↑](https://referencesource.microsoft.com)
- [Attributes](csharp/attributes.md)
  - [Nullable static analysis attributes](csharp/nullable%20static%20analysis%20attributes.md)
- [Concurrency](csharp/concurrency/concurrency.md)
  - [Asynchronous programming](csharp/concurrency/asynchronous%20programming.md)
    - [Asynchronous patterns](csharp/concurrency/asynchronous/asynchronous%20patterns.md)
    - [`Async`/`await` best practices](csharp/concurrency/asynchronous/async%20await%20best%20practices.md)
    - [`Async` lambdas](csharp/concurrency/asynchronous/async%20lambdas.md)
    - [Awaiting multiple tasks](csharp/concurrency/asynchronous/awaiting%20multiple%20tasks.md)
    - [Exceptions handling with `async` and `await`](csharp/concurrency/asynchronous/exceptions.md)
    - [Execution context](csharp/concurrency/asynchronous/execution%20context.md)
    - [Reporting task's progress](csharp/concurrency/asynchronous/reporting%20progress.md)
    - [Returning variable of `Task` type](csharp/concurrency/returning%20task.md)
  - Asynchronous streams
    - [`IAsyncEnumerable<T>` interface](csharp/concurrency/iasyncenumerable.md)
  - [Parallel programming](csharp/concurrency/parallel%20programming.md)
  - [Dataflow programming](csharp/concurrency/dataflow%20programming.md)
  - [Reactive programming](csharp/concurrency/reactive%20programming.md)
  - [Collections](csharp/concurrency/collections.md)
  - [Synchronization](csharp/concurrency/synchronization.md)
- [Constructor execution order](csharp/constructor%20execution%20order.md)
- [Covariance and contravariance](csharp/covariance%20and%20contravariance.md)
- [Equality operator](csharp/equality%20operator.md)
- [Extension methods](csharp/extension%20methods.md)
- [Garbage collector](csharp/gc.md)
- [`IEnumerable<T>`](csharp/ienumerable.md)
- [Indexers](csharp/indexers.md)
- [Keywords](csharp/keywords/keywords.md)
  - Modifiers
    - [event](csharp/keywords/event.md)
  - Contextual keywords
    - [`yield`](csharp/keywords/yield.md)
  - [`ref`, `out`, `in` (parameter modifiers)](csharp/keywords/ref%20out%20in.md)
- [Lambda expressions](csharp/lambda%20expressions.md)
- [`Object`](csharp/object.md)
- [Stopwatch](csharp/stopwatch.md)
- Types
  - [Value types](csharp/types/value%20types.md)
  - [Reference types](csharp/types/reference%20types.md)
    - Built-in reference types
      - [delegate](csharp/types/delegate.md)
    - [Nullable reference types](csharp/types/nullable%20reference%20types.md)
