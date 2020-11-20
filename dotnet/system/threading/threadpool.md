# ThreadPool class

Provides a pool of threads that can be used to execute tasks, post work items, process asynchronous I/O, wait on behalf of other threads, and process timers.

```charp
public static class ThreadPool
```

## Remarks

Many applications create threads that spend a great deal of time in the sleeping state, waiting for an event to occur. Other threads might enter a sleeping state only to be awakened periodically to poll for a change or update status information. The thread pool enables you to use threads more efficiently by providing your application with a pool of worker threads that are managed by the system.

## Links

[↑ ThreadPool Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.threadpool)
