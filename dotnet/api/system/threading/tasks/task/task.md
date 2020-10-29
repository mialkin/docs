# Task Class

Represents an asynchronous operation.

```csharp
public class Task : IAsyncResult, IDisposable
```

## Example

```csharp
using System.Threading;
using System.Threading.Tasks;

class Example
{
    static void Main()
    {
        Console.WriteLine($"Current thread: {Thread.CurrentThread.ManagedThreadId}");

        void Action(object? obj)
        {
            Console.WriteLine("Task={0}, obj={1}, Thread={2}", Task.CurrentId, obj, Thread.CurrentThread.ManagedThreadId);
        }

        Task t1 = new Task(Action, "alpha");

        Task t2 = Task.Factory.StartNew(Action, "beta");
        t2.Wait();

        t1.Start();
        Console.WriteLine("t1 has been launched. (Main Thread={0})", Thread.CurrentThread.ManagedThreadId);
        t1.Wait();

        String taskData = "delta";
        Task t3 = Task.Run(() =>
        {
            Console.WriteLine("Task={0}, obj={1}, Thread={2}", Task.CurrentId, taskData, Thread.CurrentThread.ManagedThreadId);
        });
        t3.Wait();

        Task t4 = new Task(Action, "gamma");
        t4.RunSynchronously();
        // Although the task was run synchronously, it is a good practice to wait for it if the event exceptions were thrown by the task.
        t4.Wait();
    }
}
```

## Remarks

The `Task` class represents a single operation that does not return a value and that usually executes asynchronously.

## Links

[↑ Task Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task)

[↑ Task.Run vs Task.Factory.StartNew](https://devblogs.microsoft.com/pfxteam/task-run-vs-task-factory-startnew/)