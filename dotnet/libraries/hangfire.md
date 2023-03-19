# Hangfire

[↑ Hangfire](https://www.hangfire.io) is an open-source framework that can be used to perform background processing in .NET applications.

Hangfire can be considered as a state machine for *background jobs*.

[↑ GitHub repository](https://github.com/HangfireIO/Hangfire).

## Background jobs

A **background jobs** in Hangfire looks like regular method calls. Most of its interfaces are using expression trees to define what method should be called and with what arguments.

The purpose of the method calls is to collect and serialize the following information:

- Type name, including namespace and assembly
- Method name and its parameter types
- Argument values

Serialization is performed by the `Newtonsoft.Json` package and resulting JSON is persisted in a storage making it available for other processes.

## States

Each background job has a specific **state** associated with it at every moment in time that defines how and when it will be processed. There is a bunch of built-in states like:

- `Enqueued` 
- `Scheduled`
- `Awaiting`
- `Processing`
- `Failed`
- `Succeeded`
- `Deleted`

Custom states can be implemented as well.

During background processing, background jobs are moved from one state into another with executing some side effects. So Hangfire can be considered as a state machine for background jobs. Processed background jobs end in a final state (only `Succeeded` and `Deleted` built-in states, but not the `Failed` one) and will be expired automatically after 24 hours by default.

Expiration time can be configured globally by calling the `WithJobExpirationTimeout` method. This method should be called after the `UseXXXStorage` ones:

```csharp
GlobalConfiguration.Configuration
    .UseXXXStorage(/* ... */)
    .WithJobExpirationTimeout(TimeSpan.FromHours(6));
```

## Configuration

Starting from version 1.4, `GlobalConfiguration` class is the preferred way to configure Hangfire. This is an entry point for a couple of methods, including ones from third-party storage implementations or other extensions.

## Using SQL Server for storage

SQL Server is the default storage for Hangfire — it is well known to many .NET developers and used in many project environments. It might be interesting that in the early stage of Hangfire development, Redis was used to store information about jobs, and SQL Server storage implementation was inspired by that NoSQL solution.

[↑ Using SQL Server](https://docs.hangfire.io/en/latest/configuration/using-sql-server.html).
