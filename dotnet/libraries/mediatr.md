# MediatR

[↑ MediatR](https://github.com/jbogard/MediatR) is an open source .NET library that provides simple mediator implementation for in-process messaging with no dependencies.

## Installation

```bash
dotnet add package MediatR.Extensions.Microsoft.DependencyInjection
```

## Table of contents

- [MediatR](#mediatr)
  - [Installation](#installation)
  - [Table of contents](#table-of-contents)
  - [Usage](#usage)
  - [Why do we need MediatR](#why-do-we-need-mediatr)
    - [Single responsibility](#single-responsibility)
    - [Decorators](#decorators)
  - [You probably don't need MediatR](#you-probably-dont-need-mediatr)
  - [Links](#links)

## Usage

Library registration inside `Program.cs`:

```csharp
services.AddMediatR(Assembly.GetExecutingAssembly());
```

Simple request:

```csharp
public record SimpleRequest(string Message) : IRequest<string>;
```

Simple notification:

```csharp
public record SimpleNotification(string Message) : INotification;
```

Injection of `IMediator` into controller:

```csharp
[ApiController]
[Route("[controller]")]
public class SimpleController : ControllerBase
{
    private readonly IMediator _mediator;

    public SimpleController(IMediator mediator) => _mediator = mediator;

    [HttpGet]
    [Route("SendRequest")]
    public Task<string> SendRequest(string message) => _mediator.Send(new SimpleRequest(message));

    [HttpGet]
    [Route("SendNotification")]
    public Task SendNotification(string message) => _mediator.Publish(new SimpleNotification(message));
}
```

Simple request handler:

```csharp
public class SimpleRequestHandler : IRequestHandler<SimpleRequest, string>
{
    public Task<string> Handle(SimpleRequest request, CancellationToken cancellationToken) =>
        Task.FromResult($"Request has been handled with message: {request.Message}");
}
```

Two simple notification handlers:

```csharp
public class SimpleNotificationHandler : INotificationHandler<SimpleNotification>
{
    private readonly ILogger<SimpleNotificationHandler> _logger;

    public SimpleNotificationHandler(ILogger<SimpleNotificationHandler> logger) => _logger = logger;

    public Task Handle(SimpleNotification notification, CancellationToken cancellationToken)
    {
        _logger.LogInformation("Event handled by {EventHandler} with message: {Message}",
            nameof(SimpleNotificationHandler), notification.Message);

        return Task.CompletedTask;
    }
}
```

```csharp
public class SimpleNotificationHandlerTwo : INotificationHandler<SimpleNotification>
{
    private readonly ILogger<SimpleNotificationHandlerTwo> _logger;

    public SimpleNotificationHandlerTwo(ILogger<SimpleNotificationHandlerTwo> logger) => _logger = logger;

    public Task Handle(SimpleNotification notification, CancellationToken cancellationToken)
    {
        _logger.LogInformation("Event handled by {EventHandler} with message: {Message}",
            nameof(SimpleNotificationHandlerTwo), notification.Message);

        return Task.CompletedTask;
    }
}
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

## You probably don't need MediatR

TL;DR for the impatient:

- Despite the name, MediatR does not implement the Mediator Pattern at all: it's a [↑ Command Dispatcher](https://hillside.net/plop/plop/plop2001/accepted_submissions/PLoP2001/bdupireandebfernandez0/PLoP2001_bdupireandebfernandez0_1.pdf).
- Most of the times it's used as a glorified method invocation, similarly to Service Locator, which is often an [anti-pattern](https://blog.ploeh.dk/2010/02/03/ServiceLocatorisanAnti-Pattern).
- Classes that use it are forced to depend on methods they don't use, violating the [↑ Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle). This creates an implicit, global coupling.
- They also tend to violate the [↑ Explicit Dependencies Principle](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#explicit-dependencies): instead of explicitly requiring the collaborating objects they need, they get a global accessor to the whole domain
- Domain code cannot have interfaces named after the domain-driven language
- The domain code is polluted with Send and Handle methods
- Domain classes are forced to implement interfaces defined in MediatR, ending up being coupled with a 3rd party library
- Browsing code is harder
- IntelliSense is no help
- The compiler gets confused and marks classes as unused. Workarounds are hacks.
- Invoking the handler directly is about to 50x faster and allocates way less memory than invoking it through MediatR.
- Good news is: MediatR can be easily replaced with trivial OOP techniques

For more details see [↑ "You probably don't need MediatR"](http://arialdomartini.github.io/mediatr) article.

## Links

[↑ You Probably Don't Need to Worry About MediatR](https://jimmybogard.com/you-probably-dont-need-to-worry-about-mediatr)
