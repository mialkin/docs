# Parallelism

Most servers have some parallelism built in; for example, ASP.NET will handle multiple requests in parallel. Writing parallel code on the server may still be useful in some situations (if you **know** that the number of concurrent users will always be low), but in general, parallel programming on the server would work against its built-in parallelism and therefore wouldn't provide any real benefit.

There are two forms of parallelism: *data parallelism* and *task parallelism*.

**Data parallelism** is when you have a bunch of data items to process, and the processing of each piece of data is mostly independent from the other pieces.

**Task parallelism** is when you have a pool of work to do, and each piece of work is mostly independent from the other pieces.

## Implementing data parallelism

There are a few different ways to do data parallelism. `Parallel.ForEach` is similar to a `foreach` loop and *should be used when possible*. The `Parallel` class also supports `Parallel.For`, which is similar to a for loop, and can be used if the data processing depends on the index.

Another option is PLINQ (Parallel LINQ), which provides an `AsParallel` extension method for LINQ queries. `Parallel` is more resource friendly than PLINQ; `Parallel` will play more nicely with other processes in the system, while PLINQ will (by default) attempt to spread itself over all CPUs. The downside to `Parallel` is that it's more explicit; PLINQ in many cases has more elegant code.

Regardless of the method you choose, one guideline stands out when doing parallel processing: *the chunks of work should be as independent from one another as possible*.

As long as your chunk of work is independent from all other chunks, you maximize your parallelism. As soon as you start sharing state between multiple threads, you have to synchronize access to that shared state, and your application becomes less parallel.

## Implementing task parallelism

Data parallelism is focused on processing data; task parallelism is just about doing work. At a high level, data parallelism and task parallelism are similar; "processing data" is a kind of "work". Many parallelism problems can be solved either way; it's convenient to use whichever API is more natural for the problem at hand.

`Parallel.Invoke` is one type of `Parallel` method that does a kind of
fork/join task parallelism.

The `Task` type was originally introduced for task parallelism, though these days it's also used for asynchronous programming.

Task parallelism should strive to be independent, just like data parallelism. The more independent your delegates can be, the more efficient your program can be.

## Error handling

Error handling is similar for all kinds of parallelism. Because operations are proceeding in parallel, it's possible for multiple exceptions to occur, so they are wrapped up in an `AggregateException` that's thrown to your code. This behavior is consistent across `Parallel.ForEach`, `Parallel.Invoke`, `Task.Wait`, etc.

## Thread pool

Usually, you don't have to worry about how the work is handled by the thread pool. Data and task parallelism use dynamically adjusting partitioners to divide work among worker threads. The thread pool increases its thread count as necessary. The thread pool has a single work queue, and each threadpool thread also has its own work queue. When a threadpool thread queues additional work, it sends it to its own queue first because the work is usually related to the current work item; this behavior encourages threads to work on their own work, and maximizes cache hits. If another thread doesn't have work to do, it'll steal work from another thread's queue.

As long as your tasks are not extremely short, they should work well with the default settings: *tasks should neither be extremely short, nor extremely long*.

If your tasks are too short, then the overhead of breaking up the data into tasks and scheduling those tasks on the thread pool becomes significant. If your tasks are too long, then the thread pool cannot dynamically adjust its work balancing efficiently. It's difficult to determine how short is too short and how long is too long; it really depends on the problem being solved and the approximate capabilities of the hardware. As a general rule, I try to make my tasks as short as possible without running into performance issues (you'll see your performance suddenly degrade when your tasks are too short). Even better, instead of using tasks directly, use the `Parallel` type or PLINQ. These higher-level forms of parallelism have partitioning built in to handle this automatically for you (and adjust as necessary at runtime).
