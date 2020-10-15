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
            Console.WriteLine($"CurrentThread.Name: {Thread.CurrentThread.Name}");
            Console.WriteLine($"CurrentThread.ManagedThreadId: {Thread.CurrentThread.ManagedThreadId}");
            Console.WriteLine($"CurrentThread.Priority: {Thread.CurrentThread.Priority}");
            Console.WriteLine($"CurrentThread.CurrentUICulture: {Thread.CurrentThread.CurrentUICulture}");
            Console.WriteLine($"CurrentThread.IsBackground: {Thread.CurrentThread.IsBackground}");
            Console.WriteLine($"CurrentThread.ThreadState: {Thread.CurrentThread.ThreadState}");
            Console.WriteLine($"CurrentThread.ExecutionContext: {Thread.CurrentThread.ExecutionContext}");
            Console.WriteLine($"CurrentThread.IsThreadPoolThread: {Thread.CurrentThread.IsThreadPoolThread}");
        }
    }
}
```

Ouput:

```output
CurrentThread.Name:
CurrentThread.ManagedThreadId: 1
CurrentThread.Priority: Normal
CurrentThread.CurrentUICulture:
CurrentThread.IsBackground: False
CurrentThread.ThreadState: Running
CurrentThread.ExecutionContext: System.Threading.ExecutionContext
CurrentThread.IsThreadPoolThread: False
```
