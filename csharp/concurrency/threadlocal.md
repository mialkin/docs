# `ThreadLocal<T>`

[↑ `ThreadLocal<T>`](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadlocal-1) class provides thread-local storage of data.

```csharp
public class ThreadLocal<T> : IDisposable
```

Example:

```csharp
// Thread-Local variable that yields an ID for a thread
var threadId = new ThreadLocal<int>(valueFactory: () => Environment.CurrentManagedThreadId);

// Action that prints out thread ID for the current thread
var action = () =>
{
    // If ThreadName.IsValueCreated is true, it means that we are not the
    // first action to run on this thread
    Console.WriteLine($"IsValueCreated: {threadId.IsValueCreated}. Thread ID: {threadId.Value}");
};

// Launch 8 of them. On 4 cores or less, you should see some repeat thread
Parallel.Invoke(action, action, action, action, action, action, action, action);

// Dispose when you are done
threadId.Dispose();

// Output:
// IsValueCreated: False. Thread ID: 7
// IsValueCreated: True. Thread ID: 7
// IsValueCreated: False. Thread ID: 4
// IsValueCreated: True. Thread ID: 7
// IsValueCreated: True. Thread ID: 4
// IsValueCreated: False. Thread ID: 9
// IsValueCreated: False. Thread ID: 15
// IsValueCreated: False. Thread ID: 14
```
