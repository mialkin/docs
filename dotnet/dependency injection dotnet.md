# Dependency injection in .NET

- [Dependency injection in .NET](#dependency-injection-in-net)
  - [Framework-provided services](#framework-provided-services)
  - [Service lifetimes](#service-lifetimes)
    - [Transient](#transient)
    - [Scoped](#scoped)
    - [Singleton](#singleton)
  - [Scope validation](#scope-validation)
  - [Service registration methods](#service-registration-methods)
  - [Scope scenarios](#scope-scenarios)
  - [Links](#links)

.NET provides a built-in **service container**, `IServiceProvider`, in which registration of services with the concrete types takes place. Services are typically registered at the app's start-up, and appended to an `IServiceCollection`. Once all services are added, you use `BuildServiceProvider` to create the service container.

`IServiceCollection` is a collection of `ServiceDescriptor` objects:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    string secretKey = Configuration["SecretKey"];
    var descriptor = new ServiceDescriptor(
        typeof(IMessageWriter),
        _ => new DefaultMessageWriter(secretKey),
        ServiceLifetime.Transient);

    services.Add(descriptor);
}
```


The .NET framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed, as well as injection of the service into the constructor of the class where it's used.

The service container resolves the dependencies in the graph and returns the fully resolved service. The collective set of dependencies that must be resolved is typically referred to as a **dependency tree**, **dependency graph**, or **object graph**.

## Framework-provided services

The `ConfigureServices` method in `Startup.cs` file registers services that the app uses, including platform features. Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on *how the host was configured*. For apps based on the .NET templates, the framework registers hundreds of services.

`ILogger<TCategoryName>` is an example of a framework-provided service.

## Service lifetimes

Services can be registered with one of the following lifetimes:

- Transient
- Scoped
- Singleton

### Transient

Transient lifetime services are created each time they're requested from the service container. This lifetime works best for lightweight, stateless services.

In apps that process requests, transient services are disposed at the end of the request.

### Scoped

For web applications, a scoped lifetime indicates that services are created once per client request (connection).

> When using EF Core, the `AddDbContext` extension method registers `DbContext` types with a scoped lifetime by default.

In apps that process requests, scoped services are disposed at the end of the request.

### Singleton

Singleton lifetime services are created either:

- The first time they're requested.
- By the developer, when providing an implementation instance directly to the container. This approach 
is rarely needed.

Every subsequent request of the service implementation from the dependency injection container uses the same instance.

Singleton services must be thread safe and are often used in stateless services.

In apps that process requests, singleton services are disposed when the `ServiceProvider` is disposed on application shutdown. Because memory is not released until the app is shut down, consider memory use with a singleton service.

## Scope validation

By default, in the development environment, resolving a service from another service with a longer lifetime throws an exception.

Do **not** resolve a scoped service from a singleton and be careful not to do so indirectly, for example, through a transient service. It may cause the service to have incorrect state when processing subsequent requests. It's fine to:

- Resolve a singleton service from a scoped or transient service
- Resolve a scoped service from another scoped or transient service

## Service registration methods

Service registration is generally order independent except when registering multiple implementations of the same type.

Registering a service with only an implementation type is equivalent to registering that service with the same implementation and service type. This is why multiple implementations of a service cannot be registered using the methods that don't take an explicit service type. These methods can register multiple *instances* of a service, but they will all have the same *implementation* type.

## Scope scenarios

To achieve scoping services within implementations of `IHostedService`, such as the `BackgroundService`, do **not** inject the service dependencies via constructor injection. Instead, inject `IServiceScopeFactory`, create a scope, then resolve dependencies from the scope to use the appropriate service lifetime.

## Links

[↑ Dependency injection in .NET](https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)
