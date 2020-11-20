# Thread class

Creates and controls a thread, sets its priority, and gets its status.

```csharp
public sealed class Thread : System.Runtime.ConstrainedExecution.CriticalFinalizerObject
```

## Current thread information

```csharp
using System;
using System.Threading;

class Program
{
    static void Main(string[] args)
    {
        Thread t = Thread.CurrentThread;
        Console.WriteLine("Current thread information:\n");

        Console.WriteLine($"Name: {t.Name}");
        Console.WriteLine($"ManagedThreadId: {t.ManagedThreadId}");
        Console.WriteLine($"Priority: {t.Priority}");
        Console.WriteLine($"CurrentUICulture: {t.CurrentUICulture}");
        Console.WriteLine($"IsBackground: {t.IsBackground}");
        Console.WriteLine($"ThreadState: {t.ThreadState}");
        Console.WriteLine($"ExecutionContext: {t.ExecutionContext}");
        Console.WriteLine($"IsThreadPoolThread: {t.IsThreadPoolThread}");
    }
}
```

Ouput:

```output
Current thread information:

Name:
ManagedThreadId: 1
Priority: Normal
CurrentUICulture:
IsBackground: False
ThreadState: Running
ExecutionContext: System.Threading.ExecutionContext
IsThreadPoolThread: False
```
