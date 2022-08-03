# MediatR

[↑ MediatR](https://github.com/jbogard/MediatR) is an open source .NET library that provides simple mediator implementation for in-process messaging with no dependencies.

MediatR supports request/response, commands, queries, notifications and events, synchronous and asynchronous, with intelligent dispatching via C# generic variance.

## Installation

```bash
dotnet add package MediatR
```

## Why do we need MediatR

From [↑ Why do we need MediatR?](https://ivanyakimov.blogspot.com/2021/11/why-do-we-need-mediatr.html) article:

### Single responsibility

> The next difference is more important. You see, the request handler is a class. And this whole class is responsible for performing a single operation. In the case of a service (for example, IToDoService), one method is responsible for performing one operation. This means that the service can contain many different methods, possibly related to different operations. This makes it difficult to understand the service code. On the other hand, the entire request handler class is responsible for a single operation. This makes this class smaller and easier to understand.

> It all looks nice, but the reality is slightly messier. Usually you have to support a lot of related operations (e.g. create ToDo item, update ToDo item, change status of ToDo item, ...) All these operations may require the same pieces of code. In case of service we can use private methods to do common job. But request handlers are separate classes. Of course, we can use inheritance and extract everything we need into the base class. But this brings us to the same situation, if not worse. In the case of the service, we had many methods in one class. Now we have many methods distributed across multiple classes. I'm not sure which is better.

> In other words, if you want to shoot your leg, you still have plenty of options.

### Decorators

> But there is one more serious advantage of MediatR. You see, all your request handlers implement the same interface `IRequestHandler`. It means that you can write decorators applicable to all of them. In ASP.NET Core you can use [↑ Scrutor](https://github.com/khellang/Scrutor) NuGet package for support of decorators.

> But why bother with Scrutor? MediatR provides the same functionality with pipeline behaviors. Write a class implementing IPipelineBehavior:

```csharp
class LoggingBehavior<TRequest, TResponse> : IPipelineBehavior<TRequest, TResponse>
{
    private readonly Logger _logger;

    public LoggingBehavior(Logger logger)
    {
        _logger = logger;
    }

    public async Task<TResponse> Handle(TRequest request, CancellationToken cancellationToken, RequestHandlerDelegate<TResponse> next)
    {
        try
        {
            _logger.Log($"Before execution for {typeof(TRequest).Name}");

            return await next();
        }
        finally
        {
            _logger.Log($"After execution for {typeof(TRequest).Name}");
        }
    }
}
```

> Register it:

```csharp
services.AddMediatR(typeof(Startup));
services.AddScoped(typeof(IPipelineBehavior<,>), typeof(LoggingBehavior<,>));
```

> And everything works the same way. You don't need decorators anymore. All registered pipeline behaviors will be executed with each request handler in the order they are registered.