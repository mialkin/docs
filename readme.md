# Documenting notes

> There are only two hard things in computer science: cache invalidation and naming things.
>
> — <cite>Phil Karlton</cite>

> So much complexity in software comes from trying to make one thing do two things.
>
> — <cite>Ryan Singer</cite>

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
>
> ― <cite>Martin Fowler</cite>

## Table of contents

- [Documenting notes](#documenting-notes)
  - [Table of contents](#table-of-contents)
  - [Books and blogs](#books-and-blogs)
  - [Tools](#tools)
  - [Unix](#unix)
  - [Databases](#databases)
  - [Software architecture and design](#software-architecture-and-design)
  - [Distributed systems](#distributed-systems)
  - [Programming](#programming)
  - [Testing](#testing)
  - [Networking](#networking)
  - [Security](#security)
  - [Computer science](#computer-science)
  - [Python](#python)
  - [.NET](#net)
  - [ASP.NET](#aspnet)
  - [C#](#c)
  - [Frontend](#frontend)
  - [Miscellaneous](#miscellaneous)

## Books and blogs

- [IT books and blogs](books-blogs/books-blogs.md)

## Tools

- [Amazon S3, MinIO](tools/s3.md)
- [Ansible](tools/ansible/ansible.md)
- [Docker](tools/docker/docker.md)
  - [Dockerfile](tools/docker/dockerfiles.md)
  - [Docker compose](tools/docker/docker-compose.md)
- [Docusaurus](tools/docusaurus.md)
- [Elasticsearch](tools/elasticsearch.md)
- [Git](tools/git/git.md)
- [GitLab](tools/gitlab.md)
- [Grafana](tools/grafana.md)
- [Graphviz, PlantUML, Mermaid](tools/graphviz-plantuml.md)
- [Jaeger](tools/jaeger.md)
- [Jenkins](tools/jenkins.md)
- [Kafka](tools/kafka/kafka.md)
  - [Avro](tools/kafka/avro.md)
  - [Schema Registry](tools/kafka/schema-registry.md)
- [Kubernetes](tools/kubernetes/kubernetes.md)
  - [Argo CD](tools/kubernetes/argo-cd.md)
  - [cert-manager](tools/kubernetes/cert-manager.md)
  - [Flux](tools/kubernetes/flux.md)
  - [Helm](tools/kubernetes/helm.md)
  - [Istio](tools/kubernetes/istio.md)
  - [k3s](tools/kubernetes/k3s/k3s.md)
  - [Kiali](tools/kubernetes/kiali.md)
  - [kubectl](tools/kubernetes/kubectl.md)
  - [Kustomize](tools/kubernetes/kustomize.md)
  - [microk8s](tools/kubernetes/microk8s/microk8s.md)
- [Prometheus](tools/prometheus/prometheus.md)
  - [PromQL](tools/prometheus/promql.md)
- [RabbitMQ](tools/rabbitmq/rabbitmq.md)
- [Redis](tools/redis.md)
- [Regular expressions](tools/regular-expressions/regular-expressions.md)
- [Rider](tools/rider.md)
- [SonarQube](tools/sonarqube.md)
- [Terraform](tools/terraform.md)
- [Unleash](tools/unleash.md)
- [Vault](tools/vault.md)
- [↑ Zipkin](https://zipkin.io)

## Unix

- [Environment variables](unix/environment-variables.md)
- [macOS](unix/macos/macos.md)
- [Nginx](unix/nginx.md)
- [Shell and shell scripting](unix/shell-and-shell-scripting.md)
- [Tools](unix/tools/tools.md)
  - [crontab](unix/tools/crontab.md)
  - [curl](unix/tools/curl.md)
  - [GPG](unix/tools/gpg.md)
  - [SSH](unix/tools/ssh.md)
  - [tmux](unix/tools/tmux.md)
  - [Vim](unix/tools/vim.md)
- [Ubuntu](unix/ubuntu.md)
- [User management](unix/user-managment.md)

## Databases

- [ACID](databases/acid.md)
- [Covering, composite, clustered index, cardinality, index selectivity](databases/index.md)
- [CTEs](databases/cte.md)
- [dbdiagram.io](databases/dbdiagram.md)
- [Isolation levels and read phenomena](databases/isolation-levels/isolation-levels.md)
  - [PostgreSQL isolation levels](databases/isolation-levels/isolation-levels-postgres.md)
  - [SQL Server isolation levels](databases/isolation-levels/isolation-levels-sql-server.md)
- [N + 1 query problem](databases/n-plus-one.md)
- [Natural and surrogate keys](databases/natural-and-surrogate-keys.md)
- [Online transaction processing, OLTP vs online analytical processing, OLAP](databases/oltp-vs-olap.md)
- [Optimistic vs pessimistic concurrency](databases/optimistic-vs-pessimistic-concurrency.md)
- [PL/SQL](databases/oracle/plsql.md)
- [PostgreSQL](databases/postgres/postgres.md)
  - [Client applications](databases/postgres/client-applications.md)
  - [Data organization](databases/postgres/data-organization.md)
  - [Types of indexes](databases/postgres/types-of-indexes.md)
- [Replication, sharding, partitioning](databases/replication-sharding-partitioning.md)
- [Query optimization and execution plans](databases/query-optimization-and-execution-plans.md)
- [SQL Server](databases/sqlserver/sqlserver.md)
  - [T-SQL](databases/sqlserver/tsql.md)
- [Window function](databases/window-function.md)

## Software architecture and design

- [Anti-patterns](design/anti-patterns.md)
- [Architectural decision records, ADRs](design/adr.md)
- [CAP theorem](design/cap-theorem.md)
- [Choreography vs orchestration](design/choreography-vs-orchestration.md)
- [Clean architecture](design/clean-architecture.md)
- [Composition over inheritance](design/composition-over-inheritance.md)
- [CQRS](design/cqrs.md)
- [Database per service](design/database-per-service.md)
- [DDD](design/ddd.md)
- [DRY, KISS, separation of concerns, YAGNI](design/main-principles.md)
- [Explicit dependencies principle](design/explicit-dependencies-principle.md)
- [Event sourcing](design/event-sourcing.md)
- [Loose coupling and high cohesion](design/loose-coupling-and-high-cohesion.md)
- [MapReduce](design/map-reduce.md)
- [Mission critical, business critical, business operational, office productivity](design/mission-critical-business-critical.md)
- [↑ Premature generalization](https://wiki.c2.com/?PrematureGeneralization)
- [Principle of least astonishment](design/principle-of-least-astonishment.md)
- [Repository, unit of work](design/repository-unit-of-work.md)
- Software design patterns
  - [Dependency injection](design/design-patterns/di.md)
  - [GoF book patterns](design/design-patterns/design-patterns.md)
    - Creational
      - [Abstract factory](design/design-patterns/abstract-factory.md)
      - [Builder](design/design-patterns/builder.md)
      - [Factory method](design/design-patterns/factory-method.md)
      - [Prototype](design/design-patterns/prototype.md)
      - [Singleton](design/design-patterns/singleton.md)
    - Structural
      - [Adapter](design/design-patterns/adapter.md)
      - [Bridge](design/design-patterns/bridge.md)
      - [Composite](design/design-patterns/composite.md)
      - [Decorator](design/design-patterns/decorator.md)
      - [Facade](design/design-patterns/facade.md)
      - [Flyweight](design/design-patterns/flyweight.md)
      - [Proxy](design/design-patterns/proxy.md)
    - Behavioral
      - [Chain of responsibility](design/design-patterns/chain-of-responsibility.md)
      - [Command](design/design-patterns/command.md)
      - Interpreter
      - [Iterator](design/design-patterns/iterator.md)
      - [Mediator](design/design-patterns/mediator.md)
      - [Memento](design/design-patterns/memento.md)
      - [Observer](design/design-patterns/observer.md)
      - [State](design/design-patterns/state.md)
      - [Strategy](design/design-patterns/strategy.md)
      - [Template method](design/design-patterns/template-method.md)
      - [Visitor](design/design-patterns/visitor.md)
- [Specification](design/design-patterns/specification.md)
- [SOLID](design/solid/solid.md)
  - [Single responsibility principle](design/solid/srp.md)
  - [Open-closed principle](design/solid/ocp.md)
  - [Liskov substitution principle](design/solid/lsp.md)
  - [Interface segregation principle](design/solid/isp.md)
  - [Dependency inversion principle](design/solid/dip/dip.md)
- [Technical debt](design/technical-debt.md)
- [Tell, don't ask](design/tell-dont-ask.md)
- [Transactional outbox, transactional inbox, dual-write problem](design/transactional-outbox.md)
- [↑ Utility inheritance](https://enterprisecraftsmanship.com/posts/when-inheritance-is-not-an-inheritance)
- [Vertical slice architecture](design/vertical-slice-architecture.md)

## Distributed systems

- Distributed system patterns
  - Data consistency
    - [Saga](distributed-systems/patterns/data-consistency/saga.md)
  - Reliability
    - Resiliency
      - [Retry](distributed-systems/patterns/reliability/retry.md)
- [Message delivery semantics](distributed-systems/message-delivery-semantics.md)
- [Observability: logs, metrics, traces](distributed-systems/observability/observability.md)
  - [Instrumentation guidelines](distributed-systems/observability/instrumentation-guidelines.md)
  - [OpenTelemetry](distributed-systems/observability/opentelemetry.md)
- [SLA vs SLO vs SLI](distributed-systems/sla-vs-slo-vs-sli.md)
- [↑ System design interviews](https://www.youtube.com/@IGotAnOffer-Engineering/videos)
- [Two-phase commit protocol, 2PC](distributed-systems/2pc.md)

## Programming

- [Aggregation and composition](programming/aggregation-and-composition/aggregation-and-composition.md)
- [Cyclomatic complexity](programming/cyclomatic-complexity.md)
- [↑ DTOs vs POCOs/POJOs](https://ardalis.com/dto-or-poco)
- [EditorConfig](programming/editorconfig.md)
- [Feature flags](programming/feature-flags.md)
- [Function parameters vs arguments](programming/function-parameters-vs-arguments.md)
- [Functional programming](programming/functional-programming.md)
- [Idempotence](programming/idempotence.md)
- [Inversion of control](programming/ioc/ioc.md)
- [Initiatives, epics, stories](programming/initiatives-epics-stories/initiatives-epics-stories.md)
- [Makefile](programming/makefile.md)
- [Marshalling](programming/marshalling.md)
- [Programming case types](programming/cases.md)
- [Reviewing merge requests](programming/reviewing-merge-requests.md)
- [Scrum](programming/scrum/scrum.md)
- [Serialization](programming/serialization.md)
- [Time zones vs offsets](programming/time-zones-vs-offsets.md)
- [Trunk-based development, TBD](programming/trunk-based-development.md)
- [Unicode](programming/unicode.md)
- [Upstream and downstream](programming/upstream-and-downstream/upstream-and-downstream.md)
- [YAML](programming/yaml.md)

## Testing

- [BDD, Gherkin, SpecFlow](testing/bdd.md)
- Dotnet libraries
  - [AutoFixture, Fluent Assertions, Moq, xUnit](testing/autofixture.md)
  - [↑ Playwright](https://github.com/microsoft/playwright), [↑ WireMock.Net](https://www.youtube.com/watch?v=YU3ohofu6UU)
- [k6](testing/k6/k6.md)
- [MockServer](testing/mock-server.md)
- [Test types: unit, integration, functional, end-to-end, acceptance, performance, smoke](testing/test-types.md)
  - [Integration testing](testing/integration-testing.md)
- [Test automation](testing/test-automation.md)
- [Unit testing best practices](testing/unit-testing-best-practices.md)

## Networking

- [GraphQL, gRPC](networking/graphql-grpc.md)
- [Internet, IP, TCP, OSI model](networking/internet.md)
- [Ports, sockets](networking/ports-sockets.md)
- [POST vs PUT vs PATCH](networking/post-vs-put-vs-patch.md)
- [REST, OpenAPI, egress vs ingress](networking/rest.md)
- [`ping`, `traceroute`, Wireshark](networking/tools/tools.md)

## Security

- [AD, AD FS](security/ad.md)
- [↑ DotNet Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)
  - [↑ Cross-site request forgery, XSRF or CSRF](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  - [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)
- [CORS, HSTS](security/cors.md)
- [Cryptography](security/cryptography/cryptography.md)
  - [Asymmetric cryptography](security/cryptography/asymmetric/asymmetric-cryptography.md)
    - [Generating self-signed certificate](security/cryptography/asymmetric/generating-certificate.md)
  - Symmetric cryptography
    - [AES](security/cryptography/symmetric/aes.md)
  - [TLS, mTLS](security/cryptography/tls/tls.md)
- [JWT](security/jwt.md)
- [OAuth, OpenID Connect, authentication tokens](security/oauth.md)

## Computer science

- [Algorithms](computer-science/algorithms/algorithms.md)
- [Data structures](computer-science/data-structures/data-structures.md)

## Python

- [Python](python/python.md)
  - [Syntax](python/syntax.md)

## .NET

- [Dependency injection in .NET](dotnet/dependency-injection-dotnet.md)
- [Dotnet CLI](dotnet/cli.md)
- [Dotnet tools](dotnet/dotnet-tools/dotnet-tools.md)
  - [`dotnet-dump`, `dotnet-counters`, `dotnet-trace`](dotnet/dotnet-tools/dotnet-dump-counters-trace.md)
  - [`dotnet-ef`](dotnet/dotnet-tools/dotnet-ef.md)
  - [`dotnet-format`](dotnet/dotnet-tools/dotnet-format.md)
- Dotnet libraries
  - [BenchmarkDotNet](dotnet/libraries/benchmarkdotnet.md)
  - [Bogus](dotnet/libraries/bogus.md)
  - [FluentDocker](dotnet/libraries/fluentdocker.md)
  - [FluentValidation](dotnet/libraries/fluentvalidation.md)
  - [↑ Flurl](https://github.com/tmenier/Flurl)
  - [Hangfire](dotnet/libraries/hangfire/hangfire.md)
  - [↑ Humanizer](https://github.com/Humanizr/Humanizer)
  - [MediatR](dotnet/libraries/mediatr.md)
  - [NEST](dotnet/libraries/nest.md)
  - [↑ OpenTelemetry .NET](https://github.com/open-telemetry/opentelemetry-dotnet)
  - [Polly](dotnet/libraries/polly.md)
  - [ReactiveX](dotnet/libraries/reactive/reactive-x.md)
    - [`System.Reactive` aka Rx.NET](dotnet/libraries/reactive/system-reactive.md)
- [NuGet](dotnet/nuget.md)
- [OWIN](dotnet/owin.md)
- [↑ Performance](https://github.com/adamsitnik/awesome-dot-net-performance)
- [PowerShell](dotnet/powershell.md)
  - [PowerShell scripting](dotnet/powershell-scripting.md)
- [Roslyn and Roslyn analyzers](dotnet/roslyn.md)
- [Secret Manager tool](dotnet/secret-manager.md)

## ASP.NET

- [Authentication, authorization](aspnet/authentication-authorization.md)
- [Middleware](aspnet/middleware.md)
- [SignalR, WebSocket, server-sent events, HTTP polling](aspnet/signalr.md)

## C&#35;

- [Attribute](csharp/attribute.md)
- [Closure](csharp/closure.md)
- [Concurrency](csharp/concurrency/concurrency.md)
  - [Asynchronous patterns: EAP, APM, TAP](csharp/concurrency/asynchronous-patterns.md)
    - [`async`](csharp/concurrency/tap/async.md)
    - [`IAsyncEnumerable<T>`](csharp/concurrency/tap/iasyncenumerable.md)
    - [Synchronization context, `ConfigureAwait`](csharp/concurrency/tap/synchronization-context.md)
    - [`Task`, `ValueTask`](csharp/concurrency/tap/valuetask.md)
  - [Collections](csharp/concurrency/collections/collections.md)
    - [Thread-safe collections. `BlockingCollection<T>`, `ConcurrentBag<T>`, `ConcurrentDictionary<TKey,TValue>`, `ConcurrentQueue<T>`, `ConcurrentStack<T>`](csharp/concurrency/collections/thread-safe-collections.md)
  - [Execution context](csharp/concurrency/execution-context.md)
  - [Synchronization, context switch, critical section, spinning](csharp/concurrency/synchronization/synchronization.md)
    - [`Interlocked`](csharp/concurrency/synchronization/interlocked.md)
    - [`Monitor`, `lock`, `Mutex`](csharp/concurrency/synchronization/lock.md)
    - [`ReaderWriterLockSlim`](csharp/concurrency/synchronization/readerwriterlockslim.md)
    - [`Semaphore` and `SemaphoreSlim`](csharp/concurrency/synchronization/semaphore.md)
    - [`SpinLock` and `SpinWait`](csharp/concurrency/synchronization/spinlock-and-spinwait.md)
    - Thread interaction or signaling
      - [`Barrier`](csharp/concurrency/synchronization/barrier.md)
      - [`CountdownEvent`](csharp/concurrency/synchronization/contdownevent.md)
      - [`EventWaitHandle`, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`](csharp/concurrency/synchronization/eventwaithandle.md)
  - [Thread, thread pool](csharp/concurrency/thread.md)
  - [`ThreadLocal<T>`, `AsyncLocal<T>`, `[ThreadStatic]`](csharp/concurrency/threadlocal-asynclocal.md)
- Collections
  - [↑ Which collection interface to use?](https://enterprisecraftsmanship.com/posts/which-collection-interface-to-use/)
- Exceptions
  - [↑ Exceptions for flow control: why not?](https://enterprisecraftsmanship.com/posts/exceptions-for-flow-control/)
- [Expression tree](csharp/expression-tree.md)
- [Hide method with `new`. Extend `virtual` method with `override`](csharp/virtual-override-new.md)
- [Extension method](csharp/extension-method.md)
- [Indexers](csharp/indexers.md)
- [Invariance, covariance, contravariance, variance](csharp/invariance-covariance-contravariance-variance.md)
- [Keywords](csharp/keywords/keywords.md)
  - Modifiers
    - [`event`](csharp/keywords/event.md)
  - Contextual keywords
    - [`yield`](csharp/keywords/yield.md)
  - [`ref`, `out`, `in` parameter modifiers](csharp/keywords/ref-out-in.md)
- [Lambda expression, anonymous function](csharp/lambda-expression.md)
- [LINQ](csharp/linq.md)
- [Literal](csharp/literal.md)
- [Lowering in C#](csharp/lowering.md)
- Memory
  - [Garbage collector](csharp/memory/garbage-collector.md)
  - [`IDisposable`, unmanaged resource, dispose pattern](csharp/memory/dispose-pattern.md)
  - [`WeakReference`](csharp/memory/weak-reference.md)
- [↑ Operators](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/)
  - [Equality operator `==`](csharp/operators/equality-operator.md)
  - [`stackallock`](csharp/operators/stackalloc.md)
- [Statement](csharp/statement.md)
- [`Stopwatch`, `IProgress<T>`](csharp/selected-classes.md), [↑ `PeriodicTimer`](https://www.youtube.com/watch?v=J4JL4zR_l-0)
- [`throw`, `try`, `catch`, `finally`](csharp/throw-try-catch-finally.md)
- Types
  - [Value types, `ValueType`, boxing and unboxing](csharp/types/value-types/value-types.md)
    - [↑ Built-in value types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-types#built-in-value-types)
      - [`char`](csharp/types/value-types/char.md)
    - [Enumeration types](csharp/types/value-types/enum-types.md)
    - [Structure types, `Span<T>`](csharp/types/value-types/struct-types.md), [↑ `ref` structure types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/ref-struct)
    - [↑ Tuple types](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples)
  - [Reference types, nullable reference types](csharp/types/reference-types/reference-types.md)
    - Built-in reference types
      - [`delegate`](csharp/types/reference-types/delegate.md)
      - [`object`, `dynamic`, overriding `GetHashCode` and `Equals`](csharp/types/reference-types/object.md)
      - [`string`, string interning and `string.Empty`](csharp/types/reference-types/string.md)
    - [`class`, `record`, constructors](csharp/types/reference-types/class.md), [↑ `interface`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface)

## Frontend

- CSS
  - [↑ CSS normalize and reset](https://mattbrictson.com/blog/css-normalize-and-reset)
  - [↑ 5 Simple Ways to Center a Div Horizontally and Vertically in CSS](https://bootcamp.uxdesign.cc/5-simple-ways-to-center-a-div-horizontally-and-vertically-in-css-23bbaa5f9f52)
  - [`px`, `em`, `rem`](frontend/css/px-em-rem.md)
- [Figma](frontend/figma.md)
- [JavaScript](frontend/javascript/javascript.md)
  - [Next.js](frontend/javascript/next-js.md)
  - [Node.js](frontend/javascript/node-js.md)
  - [TypeScript](frontend/javascript/typescript.md)
- [Shortcuts](frontend/shortcuts.md)

## Miscellaneous

- [Translation of terminology into Russian](translations.md)
