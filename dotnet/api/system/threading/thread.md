# Thread class

## Current thread info

```csharp
using System;
using System.Threading;

namespace csdrafts
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Current thread information:\n");
            Console.WriteLine($"Name: {Thread.CurrentThread.Name}");
            Console.WriteLine($"ManagedThreadId: {Thread.CurrentThread.ManagedThreadId}");
            Console.WriteLine($"Priority: {Thread.CurrentThread.Priority}");
            Console.WriteLine($"CurrentUICulture: {Thread.CurrentThread.CurrentUICulture}");
            Console.WriteLine($"IsBackground: {Thread.CurrentThread.IsBackground}");
            Console.WriteLine($"ThreadState: {Thread.CurrentThread.ThreadState}");
            Console.WriteLine($"ExecutionContext: {Thread.CurrentThread.ExecutionContext}");
            Console.WriteLine($"IsThreadPoolThread: {Thread.CurrentThread.IsThreadPoolThread}");
        }
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
