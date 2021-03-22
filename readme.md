# Documenting notes

- [Documenting notes](#documenting-notes)
  - [Tools](#tools)
  - [Unix](#unix)
  - [Databases](#databases)
  - [Software architecture and design](#software-architecture-and-design)
  - [Computer science](#computer-science)
  - [Web](#web)
  - [Networking](#networking)
  - [Security](#security)
  - [Microsoft](#microsoft)
  - [Python](#python)

## Tools

- [Ansible](tools/ansible/ansible.md)
- [Docker](tools/docker/docker.md)
  - [Dockerfile](tools/docker/dockerfiles.md)
  - [Docker Compose](tools/docker/docker%20compose.md)
- [Git](tools/git/git.md)
- Kubernetes
  - [Flux](tools/kubernetes/flux.md)
  - [kubectl](tools/kubernetes/kubectl.md)
  - [Creating custom Kubernetes cluster using Ansible](tools/kubernetes/creating%20cluster.md)
- [Redis](tools/redis.md)
- [Regular expressions](tools/regular%20expressions/regular%20expressions.md)
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
- [EF Core tools](db/efcore/tools.md)

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
  - [DRY, idempotency, KISS, separation of concerns, YAGNI](design/main%20principles.md)
  - [Explicit Dependencies Principle](design/explicit%20dependencies%20principle.md)
  - [SOLID](design/solid/solid.md)
    - [The Single Responsibility Principle](design/solid/srp.md)
    - [The Open-Closed Principle](design/solid/ocp.md)
    - [The Liskov Substitution Principle](design/solid/lsp.md)
    - [The Interface Segregation Principle](design/solid/isp.md)
    - [The Dependency Inversion Principle](design/solid/dip/dip.md)

## Computer science

- First-class function
- Higher-order function
- Idempotence
- Programming paradigms
  - Imperative
    - procedural
    - object-oriented
  - Declarative
    - functional
    - logic
    - mathematical
    - reactive

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

- [IP](web/ip.md)
- [TCP](web/tcp.md)

## Security

- [Asymmetric cryptography](cs/asymmetric%20cryptography.md)
- [Advanced Encryption Standard (AES)](cs/aes.md)
- [Generating self-signed certificate](cs/generating%20certificate.md)
- [TLS handshake](cs/tls%20handshake.md)
- Web
  - [↑ Cross-site request forgery (XSRF or CSRF)](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  - [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)

## Microsoft

- [.NET APIs](dotnet/api.md)
- ASP&#46;NET
  - [Host](asp.net/host.md)
- [C&#35;](csharp/csharp.md)
- Windows
  - [PowerShell](windows/powershell.md)

## Python

- [Python](python/python.md)