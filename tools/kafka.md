# Kafka

**Kafka** is a horizontally scalable, fault tolerant, and fast messaging system. It’s a pub-sub model in which various producers and consumers can write and read. It decouples source and target systems. Some of the key features are:

- Scale to hundreds of nodes
- Handle millions of messages per second
- Real-time processing (~10ms)

## Terminology

A **topic** is a specific stream of data. The topic is split into **partitions** that enable topics to be distributed across various **nodes** also called **brokers**. Topics have **offsets** per partitions. You can uniquely identify a message using its topic, partition, and offset.

Partitions enable topics to be distributed across the cluster. Partitions are a unit of parallelism for horizontal scalability. One topic can have more than one partition scaling across nodes.

Messages are assigned to partitions based on partition keys; if there are no partition keys then the partition is randomly assigned. It’s important to use the correct key to avoid hotspots.

Each message in a partition is assigned an incremental ID called an offset. Offsets are unique per partition and messages are ordered only within a partition. Messages written to partitions are immutable.

## Zookeeper

**ZooKeeper** is a centralized service for managing distributed systems. It offers hierarchical key-value store, configuration, synchronization, and name registry services to the distributed system it manages. ZooKeeper acts as ensemble layer (ties things together) and ensures high availability of the Kafka cluster. It’s important to understand that Kafka cannot work without ZooKeeper.

From the list of ZooKeeper nodes, one of the nodes is elected as a leader and the rest of the nodes follow the leader. In the case of a ZooKeeper node failure, one of the followers is elected as leader. More than one node is strongly recommended for high availability and more than 7 is not recommended.

ZooKeeper stores metadata and the current state of the Kafka cluster. For example, details like topic name, the number of partitions, replication, leader details of petitions, and consumer group details are stored in ZooKeeper. You can think of ZooKeeper like a project manager who manages resources in the project and remembers the state of the project.

Key things to remember about ZooKeeper:

- Manages list of brokers
- Elects broker leaders when a broker goes down
- Sends notifications on a new broker, new topic, deleted topic, lost brokers, etc
- Consumer offsets are not stored in ZooKeeper, only the metadata of the cluster is stored in ZooKeeper
- The leader in ZooKeeper handles all writes and follower ZooKeeper handle only reads

## Links

- [↑ Kafka Technical Overview](https://dzone.com/articles/kafka-technical-overview)
- [↑ Kafka Producer Overview](https://dzone.com/articles/kafka-producer-overview)
- [↑ Kafka Consumer Overview](https://dzone.com/articles/kafka-consumer-overview)
- [↑ Kafka Producer Delivery Semantics](https://dzone.com/articles/kafka-producer-delivery-semantics)
- [↑ Kafka Consumer Delivery Semantics](https://dzone.com/articles/kafka-consumer-delivery-semantics)
