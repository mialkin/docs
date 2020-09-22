# Chain of responsibility

The **Chain of responsibility** is a behavioral software design pattern that avoids coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.

## Participants

* **Handler**
  * defines an interface for handling requests.
  * (optional) implements the successor link.
* **ConcreteHandler**
  * handles requests it is responsible for.
  * can access its successor.
  * if the ConcreteHandler can handle the request, it does so; otherwise it forwards the request to its successor.
* **Client**
  * initiates the request to a ConcreteHandler object on the chain.

## C# implementation

```csharp
using System;

class Program
{
    public static void Main()
    {
        IHandler emailHandler = new EmailHandler(null);
        IHandler logFileHandler = new LogFileHandler(emailHandler);
        IHandler consoleHandler = new ConsoleHandler(logFileHandler);

        ILogger logger = new Logger(consoleHandler);

        Client(logger);
    }

    private static void Client(ILogger logger)
    {
        logger.Trace("TRACE");
        logger.Warning("WARNING");
        logger.Critical("CRITICAL");
    }
}

interface IHandler
{
    void Handle(string msg, LogLevel level);
}

class ConsoleHandler : IHandler
{
    private readonly IHandler _successor;

    public ConsoleHandler(IHandler successor)
    {
        _successor = successor;
    }

    public void Handle(string msg, LogLevel level)
    {
        if (level == LogLevel.Trace)
            Console.WriteLine($"Outputting to console: \"{msg}\".");

        _successor?.Handle(msg, level);
    }
}

class LogFileHandler : IHandler
{
    private readonly IHandler _successor;

    public LogFileHandler(IHandler successor)
    {
        _successor = successor;
    }

    public void Handle(string msg, LogLevel level)
    {
        if (level == LogLevel.Warning)
            Console.WriteLine($"Writing to file: \"{msg}\".");

        _successor?.Handle(msg, level);
    }
}

class EmailHandler : IHandler
{
    private readonly IHandler _successor;

    public EmailHandler(IHandler successor)
    {
        _successor = successor;
    }

    public void Handle(string msg, LogLevel level)
    {
        if (level == LogLevel.Critical)
            Console.WriteLine($"Sending email: \"{msg}\".");

        _successor?.Handle(msg, level);
    }
}

interface ILogger
{
    void Trace(string msg);
    void Warning(string msg);
    void Critical(string msg);
}

class Logger : ILogger
{
    private readonly IHandler _handler;

    public Logger(IHandler handler)
    {
        _handler = handler;
    }

    public void Trace(string msg)
    {
        _handler.Handle(msg, LogLevel.Trace);
    }

    public void Warning(string msg)
    {
        _handler.Handle(msg, LogLevel.Warning);
    }

    public void Critical(string msg)
    {
        _handler.Handle(msg, LogLevel.Critical);
    }
}

enum LogLevel
{
    Trace,
    Warning,
    Critical
}
```

Output:

```output
Outputting to console: "TRACE".
Writing to file: "WARNING".
Sending email: "CRITICAL".
```
