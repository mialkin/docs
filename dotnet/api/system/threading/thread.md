# Thread class

```csharp
using System;
using System.Threading;

namespace csdrafts
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Thread.Name: " + Thread.CurrentThread.Name);
            Console.WriteLine("Thread.ManagedThreadId: " + Thread.CurrentThread.ManagedThreadId);
            Console.WriteLine("Thread.Priority: " + Thread.CurrentThread.Priority);
            Console.WriteLine("Thread.CurrentUICulture: " + Thread.CurrentThread.CurrentUICulture);
            Console.WriteLine("Thread.IsBackground: " + Thread.CurrentThread.IsBackground);
            Console.WriteLine("Thread.ThreadState: " + Thread.CurrentThread.ThreadState);
            Console.WriteLine("Thread.ExecutionContext: " + Thread.CurrentThread.ExecutionContext);
            Console.WriteLine("Thread.IsThreadPoolThread: " + Thread.CurrentThread.IsThreadPoolThread);
        }
    }
}
```

Ouput:

```output
Thread.Name: 
Thread.ManagedThreadId: 1
Thread.Priority: Normal
Thread.CurrentUICulture: 
Thread.IsBackground: False
Thread.ThreadState: Running
Thread.ExecutionContext: System.Threading.ExecutionContext
Thread.IsThreadPoolThread: False
```