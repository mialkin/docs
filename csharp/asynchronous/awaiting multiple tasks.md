# Awaiting multiple tasks

```csharp
using System;
using System.Diagnostics;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        A a = new();
        B b = new();

        // int resultA = await a.Do();
        // int resultB = await b.Do();

        Stopwatch sw = Stopwatch.StartNew();

        Task<int> taskA = a.Do();
        Task<int> taskB = b.Do();

        Console.WriteLine(sw.ElapsedMilliseconds);

        int resultA = await taskA;
        int resultB = await taskB;

        Console.WriteLine(sw.ElapsedMilliseconds);

        Console.WriteLine($"ResultA = {resultA}");
        Console.WriteLine($"ResultB = {resultB}");
    }
}

class A
{
    public async Task<int> Do()
    {
        await Task.Delay(TimeSpan.FromSeconds(4));

        return 4;
    }
}

class B
{
    public async Task<int> Do()
    {
        await Task.Delay(TimeSpan.FromSeconds(5));

        return 5;
    }
}
```
