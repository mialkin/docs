# Semaphore

## General definition

In computer science, a **semaphore** is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multitasking operating system. A semaphore is simply a variable. This variable is used to solve critical section problems and to achieve process synchronization in the multi processing environment. A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions.

A useful way to think of a semaphore as used in the real-world system is as a record of how many units of a particular resource are available, coupled with operations to adjust that record safely (i.e., to avoid race conditions) as units are acquired or become free, and, if necessary, wait until a unit of the resource becomes available.

Semaphores which allow an arbitrary resource count are called **counting semaphores**, while semaphores which are restricted to the values 0 and 1 (or locked/unlocked, unavailable/available) are called **binary semaphores** and are used to implement locks.

The semaphore concept was invented by Dutch computer scientist Edsger Dijkstra in 1962 or 1963, when Dijkstra and his team were developing an operating system for the Electrologica X8.

## Remarks

Semaphores are of two types: local semaphores and named system semaphores. If you create a `Semaphore` object using a constructor that accepts a name, it is associated with an operating-system semaphore of that name. Named system semaphores are visible throughout the operating system, and can be used to synchronize the activities of processes. You can create multiple Semaphore objects that represent the same named system semaphore, and you can use the `OpenExisting` method to open an existing named system semaphore.
A local semaphore exists only within your process. It can be used by any thread in your process that has a reference to the local `Semaphore` object. Each `Semaphore` object is a separate local semaphore.

## Links

* [SemaphoreSlim](semaphoreslim.md)
* [↑ How do I choose between Semaphore and SemaphoreSlim?](https://stackoverflow.com/questions/4154480/how-do-i-choose-between-semaphore-and-semaphoreslim)
* [↑ Semaphore Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.semaphore)