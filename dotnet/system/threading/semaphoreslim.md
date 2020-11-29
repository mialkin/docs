# SemaphoreSlim class

Represents a lightweight alternative to [Semaphore](semaphore.md) that limits the number of threads that can access a resource or pool of resources concurrently.

## Remarks

Semaphores are of two types: local semaphores and named system semaphores. Local semaphores are local to an application, system semaphores are visible throughout the operating system and are suitable for inter-process synchronization. The `SemaphoreSlim` is a lightweight alternative to the Semaphore class that doesn't use Windows kernel semaphores. Unlike the [Semaphore](semaphore.md) class, the `SemaphoreSlim` class doesn't support named system semaphores. You can use it as a local semaphore only. The `SemaphoreSlim` class is the recommended semaphore for synchronization within a single app.

## Links

* [↑ SemaphoreSlim Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.semaphoreslim)