# SemaphoreSlim class

Represents a lightweight alternative to [Semaphore](semaphore.md) that limits the number of threads that can access a resource or pool of resources concurrently.

## Remarks

Semaphores are of two types: local semaphores and named system semaphores. Local semaphores are local to an application, system semaphores are visible throughout the operating system and are suitable for inter-process synchronization. The `SemaphoreSlim` is a lightweight alternative to the Semaphore class that doesn't use Windows kernel semaphores. Unlike the [Semaphore](semaphore.md) class, the `SemaphoreSlim` class doesn't support named system semaphores. You can use it as a local semaphore only. The `SemaphoreSlim` class is the recommended semaphore for synchronization within a single app.

## Examples

### Example 1

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static SemaphoreSlim _gate = new SemaphoreSlim(0);

    static async Task Main()
    {
        Console.WriteLine("Start");
        await _gate.WaitAsync();
        Console.WriteLine("Middle");
        Console.WriteLine("End");
    }
}
```

Outputs:

```output
Start
```

You need to pass `Start` into `SemaphoreSlim` constructor to print "Middle" and "End".

### Example 2

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static SemaphoreSlim _gate = new SemaphoreSlim(2);

    static async Task Main()
    {
        for (int i = 0; i < 10; i++)
        {
            Console.WriteLine("Start");
            await _gate.WaitAsync();
            Console.WriteLine("Middle");
            Console.WriteLine("End");
        }
    }
}
```

Outputs:

```csharp
Start
Middle
End
Start
Middle
End
Start
```

### Example 3

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static SemaphoreSlim _gate = new SemaphoreSlim(10);

    static HttpClient _client = new HttpClient
    {
        Timeout = TimeSpan.FromSeconds(5)
    };

    static void Main()
    {
        Console.WriteLine(DateTime.Now.ToUniversalTime());
        Task.WaitAll(CreateCalls().ToArray());
        Console.WriteLine(DateTime.Now.ToUniversalTime());
    }

    private static IEnumerable<Task> CreateCalls()
    {
        for (int i = 0; i < 500; i++)
        {
            yield return CallWebsite();
        }
    }

    private static async Task CallWebsite()
    {
        try
        {
            await _gate.WaitAsync();
            HttpResponseMessage response = await _client.GetAsync("https://google.com");
            _gate.Release();
            Console.WriteLine(response.StatusCode + " " + Thread.CurrentThread.ManagedThreadId);
        }
        catch (Exception e)
        {
            Console.WriteLine(e.Message);
        }
    }
}
```

## Links

* [↑ SemaphoreSlim Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim)