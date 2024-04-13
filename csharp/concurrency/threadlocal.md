# `ThreadLocal<T>`

A `ThreadLocal<T>` class provides thread-local storage of data.

```csharp
public class ThreadLocal<T> : IDisposable
```

Example:

```csharp
// Thread-local variable that yields a name for a thread
var threadName = new ThreadLocal<string>(() => "Thread " + Environment.CurrentManagedThreadId);

// Action that prints out ThreadName for the current thread
Action action = () =>
{
    // If ThreadName.IsValueCreated is true, it means that we are not the first action to run on this thread.
    bool repeat = threadName.IsValueCreated;

    Console.WriteLine("Thread name = {0} {1}", threadName.Value, repeat ? "(repeated)" : "");
};

// Launch eight of them. On 4 cores or less, you should see some repeat ThreadNames
Parallel.Invoke(action, action, action, action, action, action, action, action);

// Dispose when you are done
threadName.Dispose();

// This multithreading example can produce different outputs for each 'action' invocation and will vary with each run.
// Therefore, the example output will resemble but may not exactly match the following output (from a 4 core processor):
// Thread name = Thread 5 
// Thread name = Thread 6 
// Thread name = Thread 4 
// Thread name = Thread 6 (repeated)
// Thread name = Thread 1 
// Thread name = Thread 4 (repeated)
// Thread name = Thread 7 
// Thread name = Thread 5 (repeated)
```

## Links

[↑ `ThreadLocal<T>` class](https://learn.microsoft.com/en-us/dotnet/api/system.threading.threadlocal-1?).
