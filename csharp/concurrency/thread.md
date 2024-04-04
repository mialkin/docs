# Thread

A **process** is an executing program. An operating system uses processes to separate the applications that are being executed.

A **thread** is the basic unit to which an operating system allocates processor time.

Each thread has a *scheduling priority* and maintains a set of structures the system uses to save the *thread context* when the thread's execution is paused. The thread context includes all the information the thread needs to seamlessly resume execution, including the thread's set of CPU registers and stack.

Multiple threads can run in the context of a process. All threads of a process share its *virtual address space*. A thread can execute any part of the program code, including parts currently being executed by another thread.

By default, a .NET program is started with a single thread, often called the *primary thread*. However, it can create additional threads to execute code in parallel or concurrently with the primary thread. These threads are often called *worker threads*.
