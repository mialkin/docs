# Task\<T> class

By default, tasks execute on the current thread and delegate work to the operating system, as appropriate. Optionally, tasks can be explicitly requested to run on a separate thread via the `Task.Run` API.

Using `await` allows your application or service to perform useful work while a task is running by yielding control to its caller until the task is done. Your code does not need to rely on callbacks or events to continue execution after the task has been completed. The language and task API integration does that for you. If you’re using `Task<T>`, the `await` keyword will additionally "unwrap" the value returned when the Task is complete.
