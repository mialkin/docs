# Latency numbers

```text
L1 cache reference                           1 ns
Branch mispredict                            3 ns
L2 cache reference                           5 ns                      5x L1 cache
Mutex lock/unlock                           20 ns
Main memory reference                      100 ns                      20x L2 cache, 100x L1 cache
Read 1 MB sequentially from memory       5,000 ns        5 μs
Compress 1K bytes with Zippy             5,000 ns        5 μs
Send 1K bytes over 1 Gbps network       10,000 ns       10 μs
SSD random read                         15,000 ns       15 μs
Read 1 MB sequentially from SSD         50,000 ns       50 μs          10X memory
Round trip within same datacenter      500,000 ns      500 μs
Read 1 MB sequentially from disk     1,000,000 ns    1,000 μs    1 ms  200x memory, 20X SSD
Disk seek                            2,000,000 ns    2,000 μs    2 ms
Send packet CA->Netherlands->CA    150,000,000 ns  150,000 μs  150 ms
```

By analyzing the numbers, we get the following conclusions:

- Memory is fast but the disk is slow
- Avoid disk seeks if possible
- Simple compression algorithms are fast
- Compress data before sending it over the internet if possible
- Data centers are usually in different regions, and it takes time to send data between them

[↑ Latency numbers every programmer should know](https://gist.github.com/hellerbarde/2843375).

[↑ Interactive Latency Numbers Every Programmer Should Know](https://colin-scott.github.io/personal_website/research/interactive_latency.html).
