# Garbage collector

- [Garbage collector](#garbage-collector)
  - [Benefits](#benefits)
  - [Fundamentals of memory](#fundamentals-of-memory)
  - [Memory allocation](#memory-allocation)
  - [Memory release](#memory-release)
  - [Generations](#generations)
    - [Generation 0](#generation-0)
    - [Generation 1](#generation-1)
    - [Generation 2](#generation-2)
  - [Conditions for a garbage collection](#conditions-for-a-garbage-collection)
  - [Ephemeral generations and segments](#ephemeral-generations-and-segments)
  - [Unmanaged resources](#unmanaged-resources)
  - [See also](#see-also)

In the CLR the garbage collector (GC) serves as an automatic memory manager.

## Benefits

-   Frees developers from having to manually release memory.
-   Allocates objects on the managed heap efficiently.
-   Reclaims objects that are no longer being used, clears their memory, and keeps the memory available for future allocations. Managed objects automatically get clean content to start with, so their constructors don't have to initialize every data field.
-   Provides memory safety by making sure that an object cannot use the content of another object.

## Fundamentals of memory

-   Each process has its own, separate **virtual address space**. All processes on the same computer share the same physical memory and the page file, if there is one.
-   By default, on 32-bit computers, each process has a 2-GB user-mode virtual address space.
-   As an application developer, you work only with virtual address space and never manipulate physical memory directly. The garbage collector allocates and frees virtual memory for you on the managed heap.
-   Virtual memory can be in three states:
    -   **Free** — The block of memory has no references to it and is available for allocation.
    -   **Reserved** — The block of memory is available for your use and cannot be used for any other allocation request. However, you cannot store data to this memory block until it is committed.
    -   **Commited** — The block of memory is assigned to physical storage.
-   Virtual address space can get fragmented. This means that there are free blocks, also known as holes, in the address space. When a virtual memory allocation is requested, the virtual memory manager has to find a single free block that is large enough to satisfy that allocation request. Even if you have 2 GB of free space, an allocation that requires 2 GB will be unsuccessful unless all of that free space is in a single address block.
-   You can run out of memory if there isn't enough virtual address space to reserve or physical space to commit.

## Memory allocation

When you initialize a new process, the runtime reserves a contiguous region of address space for the process. This reserved address space is called the **managed heap** as opposed to a native heap in the operating system. There is a managed heap for each managed process. All threads in the process allocate memory for objects on the same heap.

The heap can be considered as the accumulation of two heaps: the **large object heap** and the **small object heap**. The large object heap contains objects that are 85,000 bytes and larger, which are usually arrays. It's rare for an instance object to be extremely large.

The managed heap maintains a pointer to the address where the next object in the heap will be allocated. Initially, this pointer is set to the managed heap's base address. All reference types are allocated on the managed heap. When an application creates the first reference type, memory is allocated for the type at the base address of the managed heap. When the application creates the next object, the garbage collector allocates memory for it in the address space immediately following the first object.

Allocating memory from the managed heap is faster than unmanaged memory allocation. Because the runtime allocates memory for an object by adding a value to a pointer, it's almost as fast as allocating memory from the stack. In addition, because new objects that are allocated consecutively are stored contiguously in the managed heap, an application can access the objects quickly.

## Memory release

The garbage collector's optimizing engine determines the best time to perform a collection based on the allocations being made. When the garbage collector performs a collection, it releases the memory for objects that are no longer being used by the application. It determines which objects are no longer being used by examining the **application's roots**. An application's roots include static fields, local variables and parameters on a thread's stack, and CPU registers. Each root either refers to an object on the managed heap or is set to null. The garbage collector has access to the list of active roots that the just-in-time (JIT) compiler and the runtime maintain. Using this list, the garbage collector creates a graph that contains all the objects that are reachable from the roots.

Objects that are not in the graph are unreachable from the application's roots. The garbage collector considers unreachable objects garbage and releases the memory allocated for them. During a collection, the garbage collector examines the managed heap, looking for the blocks of address space occupied by unreachable objects. As it discovers each unreachable object, it uses a memory-copying function to compact the reachable objects in memory, freeing up the blocks of address spaces allocated to unreachable objects. Once the memory for the reachable objects has been compacted, the garbage collector makes the necessary pointer corrections so that the application's roots point to the objects in their new locations. It also positions the managed heap's pointer after the last reachable object.

Memory is compacted only if a collection discovers a significant number of unreachable objects. If all the objects in the managed heap survive a collection, then there is no need for memory compaction.

To improve performance, the runtime allocates memory for large objects in a separate heap. The garbage collector automatically releases the memory for large objects. However, to avoid moving large objects in memory, this memory is usually not compacted.

## Generations

To optimize the performance of the garbage collector, the managed heap is divided into three **generations**, 0, 1, and 2, so it can handle long-lived and short-lived objects separately. The garbage collector stores new objects in generation 0. Objects created early in the application's lifetime that survive collections are promoted and stored in generations 1 and 2. Because it's faster to compact a portion of the managed heap than the entire heap, this scheme allows the garbage collector to release the memory in a specific generation rather than release the memory for the entire managed heap each time it performs a collection.

### Generation 0

This is the youngest generation and contains short-lived objects.

Newly allocated objects form a new generation of objects and are implicitly generation 0 collections. However, if they are large objects, they go on the large object heap, which is sometimes referred to as **generation 3**. Generation 3 is a physical generation that's logically collected as part of generation 2.

### Generation 1

This generation contains short-lived objects and serves as a buffer between short-lived objects and long-lived objects.

After the garbage collector performs a collection of generation 0, it compacts the memory for the reachable objects and promotes them to generation 1. The garbage collector doesn't have to reexamine the objects in generations 1 and 2 each time it performs a collection of generation 0.

If a collection of generation 0 does not reclaim enough memory for the application to create a new object, the garbage collector can perform a collection of generation 1, then generation 2. Objects in generation 1 that survive collections are promoted to generation 2.

### Generation 2

This generation contains long-lived objects. Application level singletons generally migrate to generation 2.

Objects in generation 2 that survive a collection remain in generation 2 until they are determined to be unreachable in a future collection.

Objects on the large object heap are also collected in generation 2.

## Conditions for a garbage collection

-   The system has low physical memory. This is detected by either the low memory notification from the OS or low memory as indicated by the host.
-   The memory that's used by allocated objects on the managed heap surpasses an acceptable threshold. This threshold is continuously adjusted as the process runs.
-   The `GC.Collect` method is called. In almost all cases, you don't have to call this method, because the garbage collector runs continuously. This method is primarily used for unique situations and testing.

Garbage collections occur on specific generations as conditions warrant. Collecting a generation means collecting objects in that generation and all its younger generations. A generation 2 garbage collection is also known as a **full garbage collection**, because it reclaims objects in all generations (that is, all objects in the managed heap).

The CLR continually balances two priorities: not letting an application's working set get too large by delaying garbage collection and not letting the garbage collection run too frequently.

Before a garbage collection starts, all managed threads are suspended except for the thread that triggered the garbage collection.

## Ephemeral generations and segments

Because objects in generations 0 and 1 are short-lived, these generations are known as the **ephemeral generations**.

Ephemeral generations are allocated in the **memory segment** that's known as the **ephemeral segment**. Each new segment acquired by the garbage collector becomes the new ephemeral segment and contains the objects that survived a generation 0 garbage collection. The old ephemeral segment becomes the new generation 2 segment.

The size of the ephemeral segment varies depending on whether a system is 32-bit or 64-bit and on the type of garbage collector it is running (workstation or server GC). The following table shows the default sizes of the ephemeral segment.

| Workstation/server GC           | 32-bit | 64-bit |
| ------------------------------- | ------ | ------ |
| Workstation GC                  | 16 MB  | 256 MB |
| Server GC                       | 64 MB  | 4 GB   |
| Server GC with > 4 logical CPUs | 32 MB  | 2 GB   |
| Server GC with > 8 logical CPUs | 16 MB  | 1 GB   |

The ephemeral segment can include generation 2 objects. Generation 2 objects can use multiple segments (as many as your process requires and memory allows for).

The amount of freed memory from an ephemeral garbage collection is limited to the size of the ephemeral segment. The amount of memory that is freed is proportional to the space that was occupied by the dead objects.

## Unmanaged resources

For most of the objects that your application creates, you can rely on garbage collection to automatically perform the necessary memory management tasks. However, unmanaged resources require explicit cleanup. The most common type of unmanaged resource is an object that wraps an operating system resource, such as a file handle, window handle, or network connection. Although the garbage collector is able to track the lifetime of a managed object that encapsulates an unmanaged resource, it doesn't have specific knowledge about how to clean up the resource.

When you create an object that encapsulates an unmanaged resource, it's recommended that you provide the necessary code to clean up the unmanaged resource in a public `Dispose` method. By providing a `Dispose` method, you enable users of your object to explicitly free its memory when they are finished with the object. When you use an object that encapsulates an unmanaged resource, make sure to call `Dispose` as necessary.

You must also provide a way for your unmanaged resources to be released in case a consumer of your type forgets to call `Dispose`. You can either use a safe handle (`SafeHandle` class) to wrap the unmanaged resource, or override the `Object.Finalize()` method.

## See also

[↑ Microsoft documentation — Fundamentals of garbage collection](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals)
