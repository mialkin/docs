# Latency numbers. Power of two. Availability numbers

## Latency numbers

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

## Power of two

Although data volume can become enormous when dealing with distributed systems, calculation all boils down to the basics. To obtain correct calculations, it is critical to know the data volume unit using the power of 2.

| Power | Approximate value | Full name  | Short name |
| ----- | ----------------- | ---------- | ---------- |
| 10    | 1 Thousand        | 1 Kilobyte | 1 KB       |
| 20    | 1 Million         | 1 Megabyte | 1 MB       |
| 30    | 1 Billion         | 1 Gigabyte | 1 GB       |
| 40    | 1 Trillion        | 1 Terabyte | 1 TB       |
| 50    | 1 Quadrillion     | 1 Petabyte | 1 PB       |

## Availability numbers

High availability is the ability of a system to be continuously operational for a desirably long
period of time. High availability is measured as a percentage, with 100% means a service that
has 0 downtime. Most services fall between 99% and 100%.

A service level agreement (SLA) is a commonly used term for service providers. This is an
agreement between you (the service provider) and your customer, and this agreement
formally defines the level of uptime your service will deliver. Cloud providers Amazon,
Google and Microsoft set their SLAs at 99.9% or above. Uptime is traditionally
measured in nines. The more the nines, the better.

| Availability % | Downtime per day    | Downtime per year |
| -------------- | ------------------- | ----------------- |
| 99%            | 14.40 minutes       | 3.65 days         |
| 99.9%          | 1.44 minutes        | 8.77 hours        |
| 99.99%         | 8.64 seconds        | 52.60 minutes     |
| 99.999%        | 864.00 milliseconds | 5.26 minutes      |
| 99.9999%       | 86.40 milliseconds  | 31.56 seconds     |
