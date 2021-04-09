# ValueTask\<TResult> struct

Methods may return an instance of this value type when it's likely that the result of their operations will be available synchronously and when the method is expected to be invoked so frequently that the cost of allocating a new `Task\<TResult>` for each call will be prohibitive.

Example:

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

class Example
{
    static async Task Main()
    {
        var tasks = new List<Task>();

        for (int i = 0; i < 10; i++)
        {
            var counter = i;
            Task task = Task.Run(async () =>
            {
                Print(await GetValue(counter));
            });
            tasks.Add(task);
        }

        await Task.WhenAll(tasks.ToArray());
        Console.WriteLine("Work is done");
    }

    private static async ValueTask<string> GetValue(int num)
    {
        if (num % 10 == 6)
        {
            await Task.Delay(3000);
            return "Six";
        }

        return "Not six: " + num;
    }

    private static void Print(string val) => Console.WriteLine(DateTime.Now.ToLocalTime() + " " + val);
}
```

Output:

```output
11/29/2020 23:06:19 Not six: 9
11/29/2020 23:06:19 Not six: 2
11/29/2020 23:06:19 Not six: 0
11/29/2020 23:06:19 Not six: 4
11/29/2020 23:06:19 Not six: 1
11/29/2020 23:06:19 Not six: 7
11/29/2020 23:06:19 Not six: 3
11/29/2020 23:06:19 Not six: 8
11/29/2020 23:06:19 Not six: 5
11/29/2020 23:06:22 Six
Work is done
```

As such, the default choice for any asynchronous method should be to return a `Task` or `Task<TResult>`. Only if performance analysis proves it worthwhile should a `ValueTask<TResult>` be used instead of a `Task<TResult>`. The non generic version of `ValueTask` is not recommended for most scenarios. The `CompletedTask` property should be used to hand back a successfully completed singleton in the case where a method returning a `Task` completes synchronously and successfully.

[↑ ValueTask\<TResult> Struct](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.valuetask-1)
