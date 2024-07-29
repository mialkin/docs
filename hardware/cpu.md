# CPU cache. CPU cache line

## CPU cache

A [↑ **CPU cache**](https://en.wikipedia.org/wiki/CPU_cache) is a small, high-speed memory component located on or near the CPU.

Its primary purpose is to temporarily store frequently accessed data and instructions, allowing the CPU to retrieve this information more quickly than if it had to access the RAM.

Many modern desktop, server, and industrial CPUs have at least three independent caches:

- **Instruction cache** (**I-cache**). Used to speed executable instruction fetch
- **Data cache** (**D-cache**). Used to speed data fetch and store; the data cache is usually organized as a hierarchy of more cache levels (L1, L2, etc.).
- [↑ **Translation lookaside buffer**](https://en.wikipedia.org/wiki/Translation_lookaside_buffer) (**TLB**). Used to speed virtual-to-physical address translation for both executable instructions and data.

Most CPUs have a hierarchy of multiple cache levels (L1, L2, often L3, and rarely even L4), with different instruction-specific and data-specific caches at level 1.

**L1 cache** is the smallest (typically tens of KB per core) and fastest cache, located directly on the CPU core. It typically has separate sections for instructions (I-cache) and data (D-cache).

**L2 cache** is the cache that is larger (typically few MB) and slower than L1. It can be located on the CPU core or shared among multiple cores.

**L3 cache** is the largest (typically tens of MB) and slowest among the three levels, shared across cores.

## CPU cache line

A [↑ **CPU cache line**](https://en.algorithmica.org/hpc/cpu-cache/cache-lines/) is a small, contiguous block of memory that represents the smallest unit of data that can be transferred between the cache and the main memory.

A typical cache line size is 64 bytes, although this can vary depending on the architecture. When data is fetched from main memory, an entire cache line is loaded into the cache.

The primary purpose of a cache line is to exploit spatial locality. When the CPU accesses a memory address, it is likely that nearby memory addresses will be accessed soon. By fetching an entire cache line, the CPU can reduce the number of memory accesses and improve performance.

When a CPU needs data, it first checks if the data is in the cache. If the data is found (a cache hit), it is quickly retrieved from the cache. If the data is not found (a cache miss), a cache line containing the requested data and surrounding data is fetched from the main memory and stored in the cache.

In multi-core processors, maintaining consistency across multiple caches is crucial. Cache coherence protocols ensure that all cores have a consistent view of memory, meaning that changes in one cache are propagated to others as needed.
