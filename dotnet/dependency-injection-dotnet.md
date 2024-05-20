# Dependency injection in .NET

.NET provides a built-in *service container*, [↑ `IServiceProvider`](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider), in which registration of services with the concrete types takes place. The service container resolves the dependencies in the graph and returns the fully resolved service. The collective set of dependencies that must be resolved is typically referred to as a **dependency tree**, **dependency graph**, or **object graph**.

Services are typically registered at the application's start-up, and appended to an [↑ `IServiceCollection`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection). `IServiceCollection` is a collection of `ServiceDescriptor` objects:

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

Initially, the `IServiceCollection` has services defined by the framework depending on *how the host was configured*. For apps based on the .NET templates, the framework registers hundreds of services. `ILogger<TCategoryName>` is an example of a framework-provided service.

The .NET framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed, as well as injection of the service into the constructor of the class where it's used.

[↑ Dependency injection in .NET](https://docs.microsoft.com/en-us/dotnet/core/extensions/dependency-injection).

## Table of contents

- [Dependency injection in .NET](#dependency-injection-in-net)
  - [Table of contents](#table-of-contents)
  - [Service lifetimes](#service-lifetimes)
    - [Transient](#transient)
    - [Scoped](#scoped)
    - [Singleton](#singleton)
  - [Scope validation](#scope-validation)
  - [Service registration methods](#service-registration-methods)
  - [`IServiceScopeFactory`](#iservicescopefactory)

## Service lifetimes

### Transient

Transient lifetime services are created each time they're requested from the service container. This lifetime works best for lightweight, stateless services.

### Scoped

For web applications, a scoped lifetime indicates that services are created once per client request (connection).

When using EF Core, the [↑ `AddDbContext`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers [↑ `DbContext`](https://learn.microsoft.com/en-us/dotnet/api/system.data.entity.dbcontext) types with a scoped lifetime by default.

### Singleton

Singleton lifetime services are created either:

- The first time they're requested.
- By the developer, when providing an implementation instance directly to the container

Every subsequent request of the service implementation from the dependency injection container uses the same instance.

Singleton services must be thread safe and are often used in stateless services.

In applications that process requests, singleton services are disposed when the `ServiceProvider` is disposed on application shutdown.

## Scope validation

By default, in the development environment, resolving a service from another service with a longer lifetime throws an exception.

Do **not** resolve a scoped service from a singleton and be careful not to do so indirectly, for example, through a transient service. It may cause the service to have incorrect state when processing subsequent requests. It's fine to:

- Resolve a singleton service from a scoped or transient service
- Resolve a scoped service from another scoped or transient service

## Service registration methods

Service registration is generally order independent except when registering multiple implementations of the same type.

Registering a service with only an implementation type is equivalent to registering that service with the same implementation and service type. This is why multiple implementations of a service cannot be registered using the methods that don't take an explicit service type. These methods can register multiple *instances* of a service, but they will all have the same *implementation* type.

## `IServiceScopeFactory`

To achieve scoping services within implementations of [↑ `IHostedService`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihostedservice), such as the [↑ `BackgroundService`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.backgroundservice), do **not** inject the service dependencies via constructor injection because you will get `Cannot consume scoped service 'IDemoService' from singleton 'Microsoft.Extensions.Hosting.IHostedService` exception.

Instead, inject [↑ `IServiceScopeFactory`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory), create a scope, then resolve dependencies from the scope to use the appropriate service lifetime.
