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
  - [Books](#books)
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

## Books

- [Books](books/books.md)

## Tools

- [Amazon S3](tools/amazon-s3/amazon-s3.md)
  - [MinIO](tools/amazon-s3/minio.md)
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
- [SonarQube](tools/sonarqube.md)
- [Unleash](tools/unleash.md)
- [↑ Zikpin](https://zipkin.io)

## Unix

- [Environment variables](unix/environment-variables.md)
- [macOS](unix/macos/macos.md)
- [Nginx](unix/nginx.md)
- [Shell](unix/shell/shell.md)
  - [Shell scripting](unix/shell/shell-scripting.md)
- [Tools](unix/tools/tools.md)
  - [crontab](unix/tools/crontab.md)
  - [gpg](unix/tools/gpg.md)
  - [ssh](unix/tools/ssh.md)
  - [tmux](unix/tools/tmux.md)
  - [vim](unix/tools/vim.md)
- [Ubuntu](unix/ubuntu.md)
- [User management](unix/user-managment.md)

## Databases

- [ACID](databases/acid.md)
- [CTEs](databases/cte.md)
- [dbdiagram.io](databases/dbdiagram.md)
- [Isolation levels and read phenomena](databases/isolation-levels.md)
- [N + 1 query problem](databases/n-plus-one.md)
- [Natural and surrogate keys](databases/natural-and-surrogate-keys.md)
- [Online transaction processing, OLTP vs online analytical processing, OLAP](databases/oltp-vs-olap.md)
- [Optimistic vs pessimistic concurrency](databases/optimistic-vs-pessimistic-concurrency.md)
- [PL/SQL](databases/oracle/plsql.md)
- [PostgreSQL](databases/postgres/postgres.md)
- [Query optimization and execution plans](databases/query-optimization-and-execution-plans.md)
- [SQL Server](databases/sqlserver/sqlserver.md)
  - [T-SQL](databases/sqlserver/tsql.md)
- [Window functions](databases/window-functions.md)

## Software architecture and design

- [Anti-patterns](design/anti-patterns.md)
- [Architectural decision records, ADRs](design/adr.md)
- [Choreography vs orchestration](design/choreography-vs-orchestration.md)
- [Clean architecture](design/clean-architecture.md)
- [Composition over inheritance](design/composition-over-inheritance.md)
- [CQRS](design/cqrs.md)
- [Database per service](design/database-per-service.md)
- [DDD](design/ddd.md)
- [DRY, KISS, separation of concerns, YAGNI](design/main-principles.md)
- [Explicit dependencies principle](design/explicit-dependencies-principle.md)
- [Loose coupling and high cohesion](design/loose-coupling-and-high-cohesion.md)
- [Software design patterns](design/design-patterns/design-patterns.md)
  - Creational
    - [Abstract factory](design/design-patterns/abstract-factory.md)
    - [Builder](design/design-patterns/builder.md)
    - [Dependency injection](design/design-patterns/dependency-injection.md)
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
- [SOLID](design/solid/solid.md)
  - [Single responsibility principle](design/solid/srp.md)
  - [Open-closed principle](design/solid/ocp.md)
  - [Liskov substitution principle](design/solid/lsp.md)
  - [Interface segregation principle](design/solid/isp.md)
  - [Dependency inversion principle](design/solid/dip/dip.md)
- [Technical debt](design/technical-debt.md)
- [Tell, don't ask](design/tell-dont-ask.md)
- [Transactional outbox and inbox](design/transactional-outbox-and-inbox.md)
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

- [Aggregation and composition](programming/aggregation-and-composition/aggregation-and-composition.md)
- [Cyclomatic complexity](programming/cyclomatic-complexity.md)
- [↑ DTOs vs POCOs/POJOs](https://ardalis.com/dto-or-poco)
- [EditorConfig](programming/editorconfig.md)
- [Feature flags](programming/feature-flags.md)
- [Function parameters vs arguments](programming/function-parameters-vs-arguments.md)
- [Idempotence](programming/idempotence.md)
- [Inversion of control, IoC](programming/ioc/inversion-of-control.md)
- [Initiatives, epics, stories](programming/initiatives-epics-stories/initiatives-epics-stories.md)
- [Makefile](programming/makefile.md)
- [Marshalling](programming/marshalling.md)
- [Programming case types](programming/cases.md)
- [Reviewing merge requests](programming/reviewing-merge-requests.md)
- [Scrum](programming/scrum/scrum.md)
- [Serialization](programming/serialization.md)
- [Time zones vs offsets](programming/time-zones-vs-offsets.md)
- [Upstream and downstream](programming/upstream-and-downstream/upstream-and-downstream.md)
- [YAML](programming/yaml.md)

## Testing

- [BDD](testing/bdd/bdd.md)
  - [Gherkin](testing/bdd/gherkin.md)
  - [SpecFlow](testing/bdd/specflow.md)
- Dotnet libraries
  - [AutoFixture](testing/dotnet-libraries/autofixture.md)
  - [Fluent Assertions](testing/dotnet-libraries/fluent-assertions.md)
  - [Moq](testing/dotnet-libraries/moq.md)
  - [xUnit](testing/dotnet-libraries/xunit.md)
  - [↑ WireMock.Net](https://www.youtube.com/watch?v=YU3ohofu6UU)
- [Integration testing](testing/integration-testing.md)
- [k6](testing/k6/k6.md)
- [MockServer](testing/mock-server.md)
- [Unit testing best practices](testing/unit-testing-best-practices.md)

## Networking

- [GraphQL](networking/graphql.md)
- [gRPC](networking/grpc.md)
- [Internet](networking/internet.md)
- [IP](networking/ip.md)
- [OSI](networking/osi.md)
- [Ports](networking/ports.md)
- [POST vs PUT vs PATCH](networking/post-vs-put-vs-patch.md)
- [Sockets](networking/sockets.md)
- [TCP](networking/tcp.md)
- [Wireshark](networking/wireshark/wireshark.md)

## Security

- [AD](security/ad/ad.md)
  - [AD FS](security/ad/ad-fs.md)
- [Asymmetric cryptography](security/asymmetric-cryptography/asymmetric-cryptography.md)
- [Encryption](security/encryption.md)
- [Generating self-signed certificate](security/generating-certificate.md)
- [JWT](security/jwt.md)
- [mTLS](security/mtls.md)
- [OAuth](security/oauth/oauth.md)
  - [OAuth flows](security/oauth/oauth-flows.md)
  - [OpenID Connect](security/oauth/oidc.md)
  - [Tokens](security/oauth/tokens.md)
- [OpenAPI](security/openapi.md)
- Symmetric cryptography
  - [AES](security/aes.md)
- [TLS](security/tls/tls.md)
- Web
  - [↑ Cross-site request forgery, XSRF or CSRF](https://docs.microsoft.com/en-us/aspnet/core/security/anti-request-forgery)
  - [↑ Cross-site scripting XSS](https://docs.microsoft.com/en-us/aspnet/core/security/cross-site-scripting)

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
  - [Authentication](dotnet/identity-managment/authentication.md)
  - [Authorization](dotnet/identity-managment/authorization.md)
- [NuGet](dotnet/nuget.md)
- [OWIN](dotnet/owin.md)
- [PowerShell](dotnet/powershell.md)
  - [PowerShell scripting](dotnet/powershell-scripting.md)
- [Roslyn and Roslyn analyzers](dotnet/roslyn.md)
- [Secret Manager tool](dotnet/secret-manager.md)

## ASP.NET

- [Middleware](aspnet/middleware.md)

## C&#35;

- [Attributes](csharp/attributes.md)
  - [Nullable static analysis attributes](csharp/nullable-static-analysis-attributes.md)
- [Concurrency](csharp/concurrency/concurrency.md)
  - [Asynchronous programming](csharp/concurrency/asynchronous-programming.md)
    - [Asynchronous patterns](csharp/concurrency/asynchronous/asynchronous-patterns.md)
    - [`Async`/`await` best practices](csharp/concurrency/asynchronous/async-await-best-practices.md)
    - [`Async` lambdas](csharp/concurrency/asynchronous/async-lambdas.md)
    - [Awaiting multiple tasks](csharp/concurrency/asynchronous/awaiting-multiple-tasks.md)
    - [Exceptions handling with `async` and `await`](csharp/concurrency/asynchronous/exceptions.md)
    - [Execution context](csharp/concurrency/asynchronous/execution-context.md)
    - [Reporting task's progress](csharp/concurrency/asynchronous/reporting-progress.md)
    - [Returning variable of `Task` type](csharp/concurrency/asynchronous/returning-task.md)
    - [`ValueTask<T>`](csharp/concurrency/asynchronous/valuetask.md)
  - Asynchronous streams
    - [`IAsyncEnumerable<T>` interface](csharp/concurrency/iasyncenumerable.md)
  - [Parallel programming](csharp/concurrency/parallel-programming.md)
  - [Dataflow programming](csharp/concurrency/dataflow-programming.md)
  - [Reactive programming](csharp/concurrency/reactive-programming.md)
  - [Cancellation](csharp/concurrency/cancellation.md)
  - [Collections](csharp/concurrency/collections/collections.md)
    - Thread-safe collections
      - `BlockingCollection<T>`, `ConcurrentBag<T>`, `ConcurrentDictionary<TKey,TValue>`, `ConcurrentQueue<T>`, `ConcurrentStack<T>`
    - [When to use a thread-safe collection](csharp/concurrency/collections/when-to-use-thread-safe.md)
    - [↑ Which collection interface to use?](https://enterprisecraftsmanship.com/posts/which-collection-interface-to-use/)
  - [Synchronization](csharp/concurrency/synchronization.md)
  - Synchronization primitives
    - [`Interlocked`](csharp/concurrency/synchronization-primitives/interlocked.md)
    - [`Monitor`](csharp/concurrency/synchronization-primitives/monitor.md)
    - [`Mutex`](csharp/concurrency/synchronization-primitives/mutex.md)
    - [`ReaderWriterLockSlim`](csharp/concurrency/synchronization-primitives/readerwriterlockslim.md)
    - [`Semaphore` and `SemaphoreSlim`](csharp/concurrency/synchronization-primitives/semaphore.md)
    - [`SpinLock` and `SpinWait`](csharp/concurrency/synchronization-primitives/spinlock-and-spinwait.md)
    - Thread interaction or signaling
      - [`Barrier`](csharp/concurrency/synchronization-primitives/barrier.md)
      - [`CountdownEvent`](csharp/concurrency/synchronization-primitives/contdownevent.md)
      - [`EventWaitHandle`, `AutoResetEvent`, `ManualResetEvent`, `ManualResetEventSlim`](csharp/concurrency/synchronization-primitives/eventwaithandle.md)
  - [Threads and threading](csharp/concurrency/threads-and-threading.md)
  - Exercises
    - [Downloading web pages](csharp/concurrency/exercises/downloading-pages.md)
- [Covariance and contravariance](csharp/covariance-and-contravariance.md)
- [Expression tree](csharp/expression-tree.md)
- [Extension methods](csharp/extension-methods.md)
  - [Extension methods guidelines](csharp/extension-methods-guidelines.md)
- [`IEnumerable<T>`](csharp/ienumerable.md)
- [Indexers](csharp/indexers.md)
- [Keywords](csharp/keywords/keywords.md)
  - Modifiers
    - [`event`](csharp/keywords/event.md)
  - Contextual keywords
    - [`yield`](csharp/keywords/yield.md)
  - [`ref`, `out`, `in` parameter modifiers](csharp/keywords/ref-out-in.md)
- Selected classes
  - [`HttpClient`](csharp/libraries/httpclient.md)
  - [`Stopwatch`](csharp/libraries/stopwatch.md)
- Memory
  - [Garbage collector](csharp/memory/garbage-collector.md)
  - [`GC`](csharp/memory/gc.md)
  - [`WeakReference`](csharp/memory/weak-reference.md)
  - [Dispose pattern](csharp/memory/dispose-pattern.md)
- Operators and expressions
  - [Equality operator](csharp/operators/equality-operator.md)
  - [Lambda expressions](csharp/operators/lambda-expressions.md)
- Types
  - [Value types](csharp/types/value-types.md)
    - [Boxing and unboxing](csharp/types/boxing-and-unboxing.md)
    - [Enumeration types](csharp/types/enum.md)
    - [Structure types](csharp/types/struct.md)
    - Tuple types
    - [`ValueType` class](csharp/types/value-type-class.md)
  - [Reference types](csharp/types/reference-types.md)
    - Built-in reference types
      - [`delegate`](csharp/types/delegate.md)
      - [`object`](csharp/types/object.md)
        - [Constructors](csharp/types/constructors.md)
        - [Why override `GetHashCode` when `Equals` is overridden](csharp/types/override-gethashcode.md)
      - [`record`](csharp/types/record.md)
      - [`string`](csharp/types/string.md)
    - [Nullable reference types](csharp/types/nullable-reference-types.md)

## Miscellaneous

- [Application shortcuts](shortcuts.md)
- [Translation of terminology into Russian](translations.md)
