# Documenting notes

> There are only two hard things in computer science: cache invalidation and naming things.
>
> — <cite>Phil Karlton</cite>

> So much complexity in software comes from trying to make one thing do two things.
>
> — <cite>Ryan Singer</cite>

## Table of contents

- [Documenting notes](#documenting-notes)
  - [Table of contents](#table-of-contents)
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
  - [Miscellaneous](#miscellaneous)

## Tools

- [Amazon S3](tools/amazon-s3/amazon-s3.md)
  - [MinIO](tools/amazon-s3/minio.md)
- [Ansible](tools/ansible/ansible.md)
- [Docker](tools/docker/docker.md)
  - [Dockerfile](tools/docker/dockerfiles.md)
  - [Docker compose](tools/docker/docker%20compose.md)
- [Elasticsearch](tools/elasticsearch.md)
- [Git](tools/git/git.md)
- [GitLab](tools/gitlab.md)
- [Grafana](tools/grafana.md)
- [Graphviz, PlantUML, Mermaid](tools/graphviz%20plantuml.md)
- [Jaeger](tools/jaeger.md)
- [Jenkins](tools/jenkins.md)
- [Kafka](tools/kafka/kafka.md)
  - [Avro](tools/kafka/avro.md)
  - [Schema Registry](tools/kafka/schema%20registry.md)
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
- [MC](tools/mc.md)
- [Prometheus](tools/prometheus/prometheus.md)
  - [PromQL](tools/prometheus/promql.md)
- [RabbitMQ](tools/rabbitmq/rabbitmq.md)
- [Redis](tools/redis.md)
- [Regular expressions](tools/regular%20expressions/regular%20expressions.md)
- [SonarQube](tools/sonarqube.md)
- [↑ Zikpin](https://zipkin.io)

## Unix

- [Environment variables](unix/environment%20variables.md)
- [macOS](unix/macos/macos.md)
- [Ubuntu](unix/ubuntu.md)
- [Nginx](unix/nginx.md)
- [Shell](unix/shell/shell.md)
  - [Shell scripting](unix/shell/shell%20scripting.md)
- [Tools](unix/tools.md)
  - [crontab](unix/crontab.md)
  - [gpg](unix/gpg.md)
  - [ssh](unix/ssh.md)
  - [tmux](unix/tmux.md)
  - [vim](unix/vim.md)
- [User management](unix/user%20managment.md)

## Databases

- [ACID](databases/acid.md)
- [CTEs](databases/cte.md)
- [Isolation levels and read phenomena](databases/isolation%20levels.md)
- [N + 1 query problem](databases/n%20plus%20one.md)
- [Natural and surrogate keys](databases/natural%20and%20surrogate%20keys.md)
- [PL/SQL](databases/oracle/plsql.md)
- [PostgreSQL](databases/postgres/postgres.md)
- [SQL Server](databases/sqlserver/sqlserver.md)
  - [T-SQL](databases/sqlserver/tsql.md)
- [Window functions](databases/window%20functions.md)

## Software architecture and design

- [Architectural decision records, ADRs](design/adr.md)
- [Anti-patterns](design/anti-patterns.md)
- [Clean architecture](design/clean%20architecture.md)
- [Composition over inheritance](design/composition%20over%20inheritance.md)
- [DDD](design/ddd.md)
- [Software design patterns](design/design%20patterns/design%20patterns.md)
  - Creational
    - [Abstract Factory](design/design%20patterns/abstract%20factory.md)
    - [Builder](design/design%20patterns/builder.md)
    - [Dependency injection](design/design%20patterns/dependency%20injection.md)
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
- [DRY, KISS, separation of concerns, YAGNI](design/main%20principles.md)
- [Explicit dependencies principle](design/explicit%20dependencies%20principle.md)
- [Loose coupling and high cohesion](design/loose%20coupling%20and%20high%20cohesion.md)
- [SOLID](design/solid/solid.md)
  - [Single responsibility principle](design/solid/srp.md)
  - [Open-closed principle](design/solid/ocp.md)
  - [Liskov substitution principle](design/solid/lsp.md)
  - [Interface segregation principle](design/solid/isp.md)
  - [Dependency inversion principle](design/solid/dip/dip.md)
- [Technical debt](design/technical%20debt.md)
- [Vertical slice architecture](design/vertical-slice-architecture.md)

## Distributed systems

- Distributed system patterns
  - Reliability
    - Resiliency
      - [Retry](distributed-systems/patterns/reliability/retry.md)
- [Message delivery semantics](distributed-systems/message-delivery-semantics.md)
- [Observability: logs, metrics, traces](distributed-systems/observability/observability.md)
  - [Instrumentation guidelines](distributed-systems/observability/instrumentation-guidelines.md)
  - [OpenTelemetry](distributed-systems/observability/opentelemetry.md)
- [Transactional outbox and inbox](distributed-systems/transactional-outbox-and-inbox.md)
- [Two-phase commit protocol, 2PC](distributed-systems/2pc.md)

## Programming

- [Aggregation and composition](programming/aggregation%20and%20composition/aggregation%20and%20composition.md)
- [Cyclomatic complexity](programming/cyclomatic%20complexity.md)
- [↑ DTOs vs POCOs/POJOs](https://ardalis.com/dto-or-poco)
- [EditorConfig](programming/editorconfig.md)
- [Function parameters vs arguments](programming/function%20parameters%20vs%20arguments.md)
- [Idempotence](programming/idempotence.md)
- [Inversion of control (IoC)](programming/ioc/inversion%20of%20control.md)
- [Makefile](programming/makefile.md)
- [Programming case types](programming/cases.md)
- [Reviewing merge requests](programming/reviewing-merge-requests.md)
- [Scrum](programming/scrum/scrum.md)
- [Serialization](programming/serialization.md)
- [Time zones vs offsets](programming/time%20zones%20vs%20offsets.md)
- [YAML](programming/yaml.md)

## Testing

- [↑ Allure framework](https://docs.qameta.io/allure)
- [BDD](testing/bdd/bdd.md)
  - [Gherkin](testing/bdd/gherkin.md)
  - [SpecFlow](testing/bdd/specflow.md)
- Dotnet libraries
  - [AutoFixture](testing/dotnet-libraries/autofixture.md)
  - [↑ FluentAssertions](https://fluentassertions.com/introduction)
  - [Moq](testing/dotnet-libraries/moq.md)
  - [xUnit](testing/dotnet-libraries/xunit.md)
  - [↑ WireMock.Net](https://www.youtube.com/watch?v=YU3ohofu6UU)
- [Integration testing](testing/integration%20testing.md)
- [k6](testing/k6/k6.md)
- [MockServer](testing/mock-server.md)
- [Unit testing best practices](testing/unit%20testing%20best%20practices.md)

## Networking

- [GraphQL](networking/graphql.md)
- [gRPC](networking/grpc.md)
- [Internet](networking/internet.md)
- [IP](networking/ip.md)
- [Ports](networking/ports.md)
- [Sockets](networking/sockets.md)
- [TCP](networking/tcp.md)
- [Wireshark](networking/wireshark/wireshark.md)

## Security

- [AD](security/ad/ad.md)
  - [AD FS](security/ad/ad%20fs.md)
- [Asymmetric cryptography](security/asymmetric%20cryptography/asymmetric%20cryptography.md)
- [Encryption](security/encryption.md)
- [Generating self-signed certificate](security/generating%20certificate.md)
- [JWT](security/jwt.md)
- [mTLS](security/mtls.md)
- [OAuth](security/oauth/oauth.md)
  - [OAuth flows](security/oauth/oauth%20flows.md)
  - [OpenID Connect](security/oauth/oidc.md)
  - [Tokens](security/oauth/tokens.md)
- [OpenAPI](security/openapi.md)
- Symmetric cryptography
  - [AES](security/aes.md)
- [TLS](security/tls/tls.md)
- Web
  - [↑ Cross-site request forgery (XSRF or CSRF)](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  - [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)

## Computer science

- [Algorithms](computer%20science/algorithms/algorithms.md)
- [Data structures](computer%20science/data%20structures/data%20structures.md)

## Python

- [Python](python/python.md)
  - [Syntax](python/syntax.md)

## .NET

- [Dependency injection in .NET](dotnet/dependency%20injection%20dotnet.md)
- [Dotnet CLI](dotnet/cli.md)
- [Dotnet tools](dotnet/dotnet-tools/dotnet-tools.md)
  - [`dotnet-ef` tool](dotnet/dotnet-tools/dotnet-ef.md)
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
  - [↑ YARP](https://github.com/microsoft/reverse-proxy)
- Identity management
  - [Authentication](dotnet/identity%20managment/authentication.md)
  - [Authorization](dotnet/identity%20managment/authorization.md)
- [OWIN](dotnet/owin.md)
- [PowerShell](dotnet/powershell.md)
  - [PowerShell scripting](dotnet/powershell%20scripting.md)
- [Roslyn and Roslyn analyzers](dotnet/roslyn.md)
- [Secret Manager tool](dotnet/secret%20manager.md)

## ASP.NET

- [Middleware](aspnet/middleware.md)

## C&#35;

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
    - [Returning variable of `Task` type](csharp/concurrency/asynchronous/returning%20task.md)
    - [`ValueTask<T>`](csharp/concurrency/asynchronous/valuetask.md)
  - Asynchronous streams
    - [`IAsyncEnumerable<T>` interface](csharp/concurrency/iasyncenumerable.md)
  - [Parallel programming](csharp/concurrency/parallel%20programming.md)
  - [Dataflow programming](csharp/concurrency/dataflow%20programming.md)
  - [Reactive programming](csharp/concurrency/reactive%20programming.md)
  - [Cancellation](csharp/concurrency/cancellation.md)
  - [Collections](csharp/concurrency/collections/collections.md)
    - Thread-safe collections
      - `BlockingCollection<T>`, `ConcurrentBag<T>`, `ConcurrentDictionary<TKey,TValue>`, `ConcurrentQueue<T>`, `ConcurrentStack<T>`
    - [When to use a thread-safe collection](csharp/concurrency/collections/when%20to%20use%20thread-safe.md)
    - [↑ Which collection interface to use?](https://enterprisecraftsmanship.com/posts/which-collection-interface-to-use/)
  - [Synchronization](csharp/concurrency/synchronization.md)
  - Synchronization primitives
    - [`Interlocked`](csharp/concurrency/synchronization%20primitives/interlocked.md)
    - [`Monitor`](csharp/concurrency/synchronization%20primitives/monitor.md)
    - [`Mutex`](csharp/concurrency/synchronization%20primitives/mutex.md)
    - [`ReaderWriterLockSlim`](csharp/concurrency/synchronization%20primitives/readerwriterlockslim.md)
    - [`Semaphore` and `SemaphoreSlim`](csharp/concurrency/synchronization%20primitives/semaphore.md)
    - [`SpinLock` and `SpinWait`](csharp/concurrency/synchronization%20primitives/spinlock%20and%20spinwait.md)
    - Thread interaction or signaling
      - [`Barrier`](csharp/concurrency/synchronization%20primitives/barrier.md)
      - [`CountdownEvent`](csharp/concurrency/synchronization%20primitives/contdownevent.md)
      - [`EventWaitHandle`, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`](csharp/concurrency/synchronization%20primitives/eventwaithandle.md)
  - [Threads and threading](csharp/concurrency/threads%20and%20threading.md)
  - Exercises
    - [Downloading web pages](csharp/concurrency/exercises/downloading%20pages.md)
- [Covariance and contravariance](csharp/covariance%20and%20contravariance.md)
- [Extension methods](csharp/extension%20methods.md)
  - [Extension methods guidelines](csharp/extension%20methods%20guidelines.md)
- [`IEnumerable<T>`](csharp/ienumerable.md)
- [Indexers](csharp/indexers.md)
- [Keywords](csharp/keywords/keywords.md)
  - Modifiers
    - [`event`](csharp/keywords/event.md)
  - Contextual keywords
    - [`yield`](csharp/keywords/yield.md)
  - [`ref`, `out`, `in` (parameter modifiers)](csharp/keywords/ref%20out%20in.md)
- Selected classes
  - [`HttpClient`](csharp/libraries/httpclient.md)
  - [`Stopwatch`](csharp/libraries/stopwatch.md)
- Memory
  - [Garbage collector](csharp/memory/garbage%20collector.md)
  - [`GC`](csharp/memory/gc.md)
  - [`WeakReference`](csharp/memory/weak%20reference.md)
  - [Dispose pattern](csharp/memory/dispose%20pattern.md)
- Operators and expressions
  - [Equality operator](csharp/operators/equality%20operator.md)
  - [Lambda expressions](csharp/operators/lambda%20expressions.md)
- Types
  - [Value types](csharp/types/value%20types.md)
    - [Boxing and unboxing](csharp/types/boxing%20and%20unboxing.md)
    - [Enumeration types](csharp/types/enum.md)
    - [Structure types](csharp/types/struct.md)
    - Tuple types
    - [`ValueType` class](csharp/types/value%20type%20class.md)
  - [Reference types](csharp/types/reference%20types.md)
    - Built-in reference types
      - [`delegate`](csharp/types/delegate.md)
      - [`object`](csharp/types/object.md)
        - [Constructors](csharp/types/constructors.md)
        - [Why override `GetHashCode` when `Equals` is overriden](csharp/types/override%20gethashcode.md)
      - [`record`](csharp/types/record.md)
      - [`string`](csharp/types/string.md)
    - [Nullable reference types](csharp/types/nullable%20reference%20types.md)

## Miscellaneous

- [Application shortcuts](shortcuts.md)
- [Translation of terminology into Russian](translations.md)
