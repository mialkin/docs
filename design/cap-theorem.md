# CAP theorem

The **CAP theorem**, also known as **Brewer's theorem**, is a fundamental concept in distributed systems that describes the trade-offs among three key properties: consistency, availability, and partition tolerance.

The theorem was proposed by computer scientist Eric Brewer in 2000.

1. **Consistency, C:** This property ensures that all nodes in a distributed system have the same data at the same time. In other words, when a client reads from the system, it receives the most recent write. Achieving strong consistency may lead to delays in system responses because all nodes need to be updated before a read operation is considered successful.

2. **Availability, A:** Availability refers to the ability of a distributed system to provide a response, even in the presence of faults or failures. In a highly available system, every request receives a response, without guaranteeing that it contains the most recent write.

3. **Partition Tolerance, P:** Partition tolerance deals with the system's ability to continue operating despite network partitions, communication failures, that may occur between nodes in a distributed system. In practical terms, it means the system can function even if some nodes are unreachable or if the communication between nodes is delayed.

According to the CAP theorem, it is impossible for a distributed system to simultaneously provide all three of these properties. Instead, a distributed system must choose to prioritize two out of the three. The theorem is often visualized as a triangle, where each corner represents one of the properties, and a distributed system can position itself within this triangle based on its design choices.

Here are the three common scenarios described by the CAP theorem:

1. **CA systems, consistent and available:** These systems prioritize consistency and availability but may sacrifice partition tolerance. They operate well in environments where network partitions are rare.

2. **CP systems, consistent and partition-tolerant:** These systems prioritize consistency and partition tolerance but may sacrifice availability during network partitions. They ensure that data remains consistent across nodes even when communication is disrupted.

3. **AP systems, available and partition-tolerant:** These systems prioritize availability and partition tolerance but may sacrifice consistency. They can provide responses even in the presence of network partitions but may not guarantee the most recent data in every read operation.

The CAP theorem provides a framework for understanding the inherent trade-offs in distributed systems architecture and helps guide design decisions based on the specific requirements and constraints of a given application or use case.