# `Task` examples

Execute method concurrently using all available CPU threads:

```csharp
var numberOfCores = Environment.ProcessorCount;

var tasks = new Task[numberOfCores];
for (var i = 0; i < numberOfCores; i++)
{
    tasks[i] = ExecuteMethodConcurrently();
}

Console.ReadLine();

async Task ExecuteMethodConcurrently()
{
    while (true)
    {
        await GoAsync().ConfigureAwait(false);
    }
}

async Task GoAsync()
{
    await Task.Delay(100);
    Console.WriteLine("Executing method on thread: " + Environment.CurrentManagedThreadId);
}
```

Start all tasks immediately:

```csharp
var tasks = new List<Task> { DoAsync(1), DoAsync(2), DoAsync(3) };
Console.ReadLine();

async Task DoAsync(int id)
{
    Console.WriteLine($"Task {id} started.");
    await Task.Yield();
}
```
