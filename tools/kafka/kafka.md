# Kafka

**Kafka** is a horizontally scalable, fault tolerant, and fast messaging system. It's a pub-sub model in which various producers and consumers can write and read. It decouples source and target systems.

Some of the key Kafka features:

- Scale to hundreds of nodes
- Handle millions of messages per second
- Real-time processing (~10ms)

## Table of contents

- [Kafka](#kafka)
  - [Table of contents](#table-of-contents)
  - [Terminology and components](#terminology-and-components)
    - [Zookeeper](#zookeeper)
    - [Broker](#broker)
    - [Replication](#replication)
  - [Kafka producers](#kafka-producers)
  - [Running Kafka in Docker](#running-kafka-in-docker)
  - [Kafka consumer auto offset reset](#kafka-consumer-auto-offset-reset)
  - [Exactly-Once Semantics](#exactly-once-semantics)
  - [The Transactional Outbox Pattern](#the-transactional-outbox-pattern)
  - [Repository](#repository)
  - [Links](#links)

## Terminology and components

A **topic** is a message streams with one or more *partitions*. The topic is split into **partitions** that enable topics to be distributed across various **nodes** also called **brokers**. Topics have **offsets** per partitions. You can uniquely identify a message using its topic, partition, and offset.

Partitions enable topics to be distributed across the cluster. Partitions are a unit of parallelism for horizontal scalability. One topic can have more than one partition scaling across nodes.

Messages are assigned to partitions based on partition keys; if there are no partition keys then the partition is randomly assigned. It's important to use the correct key to avoid hotspots.

Each message in a partition is assigned an incremental ID called an offset. Offsets are unique per partition and messages are ordered only within a partition. Messages written to partitions are immutable.

### Zookeeper

**ZooKeeper** is a centralized service for managing distributed systems. It offers hierarchical key-value store, configuration, synchronization, and name registry services to the distributed system it manages.

> [↑ Apache Kafka Needs No Keeper: Removing the Apache ZooKeeper Dependency](https://www.confluent.io/blog/removing-zookeeper-dependency-in-kafka)

From the list of ZooKeeper nodes, one of the nodes is elected as a leader and the rest of the nodes follow the leader. In the case of a ZooKeeper node failure, one of the followers is elected as leader. More than one node is strongly recommended for high availability and more than 7 is not recommended.

ZooKeeper stores metadata and the current state of the Kafka cluster. For example, details like topic name, the number of partitions, replication, leader details of petitions, and consumer group details are stored in ZooKeeper. You can think of ZooKeeper like a project manager who manages resources in the project and remembers the state of the project.

Key things to remember about ZooKeeper:

- Manages list of *brokers*, i.e. Kafka nodes
- Elects broker leaders when a broker goes down
- Sends notifications on a new broker, new topic, deleted topic, lost brokers, etc
- Consumer offsets are not stored in ZooKeeper, only the metadata of the cluster is stored in ZooKeeper
- The leader in ZooKeeper handles all writes and follower ZooKeeper handle only reads

### Broker

A **broker** is a single Kafka node that is managed by ZooKeeper. A set of brokers form a Kafka **cluster**. Topics that are created in Kaka are distributed across brokers based on the partition, replication, and other factors. When a broker node fails based on the state stored in ZooKeeper it automatically rebalances the cluster and if a leader partition is lost then one of the follower partitions is elected as the leader.

### Replication

A **replication** is making a copy of a partition available in another broker. Replication enables Kafka to be fault tolerant. When a partition of the topic is available in multiple brokers then one of the partitions in a broker is elected as the leader and the rest of the replications of the partition are followers.

Replication enables Kafka to be fault tolerant even when a broker is down. For example, Topic B partition 0 is stored in both broker 0 and broker 1. Both producers and consumers are served only by the leader. In case of a broker failure the partition from another broker is elected as a leader and it starts serving the producers and consumer groups. Replica partitions that are in sync with the leader are flagged as **ISR** (in sync replica).

## Kafka producers

The primary role of a Kafka producer is to take producer properties, record them as inputs, and write them to an appropriate Kafka broker. Producers serialize, partition, compress, and load balance data across brokers based on partitions.

Some of the producer properties are: `bootstrap servers`, ACKs, `batch.size`, `linger.ms`, `key.serializer`, `value.serializer`, and many more.

A message that should be written to Kafka is referred to as a producer record. A producer record contains the name of the topic it should be written to and the value of the record. Other fields like partition, timestamp, and key are optional.

## Running Kafka in Docker

```bash
docker-compose up -d
```

Contents of `docker-compose.yaml` file:

```yaml
version: "3.9"

services:
  schema-registry:
    image: confluentinc/cp-schema-registry:5.5.3
    ports:
      - "18081:8081"
    environment:
      SCHEMA_REGISTRY_HOST_NAME: schema-registry
      SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: "PLAINTEXT://kafka:9092"
      SCHEMA_REGISTRY_LISTENERS: http://0.0.0.0:8081
    depends_on:
      - kafka
      - zookeeper

  kafka:
    image: confluentinc/cp-kafka:5.5.3
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      # Kafka is available on port 9092 inside Docker network and on port 9093 at localhost.
      KAFKA_ADVERTISED_LISTENERS: CLIENT://kafka:9092,EXTERNAL://localhost:9093
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: CLIENT:PLAINTEXT,EXTERNAL:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: CLIENT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_SCHEMA_REGISTRY_URL: "schema-registry:8081"
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "true"
    ports:
      - "9092:9092"
      - "9093:9093"
    depends_on:
      - zookeeper

  zookeeper:
    image: confluentinc/cp-zookeeper:5.5.3
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181

  kafka-ui:
    image: provectuslabs/kafka-ui:latest
    ports:
      - "1000:8080"
    environment:
      KAFKA_CLUSTERS_0_NAME: local
      KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:9092
    depends_on:
      - kafka
```

## Kafka consumer auto offset reset

[↑ Kafka Consumer Auto Offset Reset](https://medium.com/lydtech-consulting/kafka-consumer-auto-offset-reset-d3962bad2665)

## Exactly-Once Semantics

https://medium.com/lydtech-consulting/kafka-transactions-part-1-exactly-once-messaging-9949350281ff

## The Transactional Outbox Pattern

https://medium.com/lydtech-consulting/kafka-idempotent-consumer-transactional-outbox-74b304815550

## Repository

<https://github.com/mialkin/kafka>

## Links

- [↑ Kafka Technical Overview](https://dzone.com/articles/kafka-technical-overview)
- [↑ Kafka Producer Overview](https://dzone.com/articles/kafka-producer-overview)
- [↑ Kafka Consumer Overview](https://dzone.com/articles/kafka-consumer-overview)
- [↑ Kafka Producer Delivery Semantics](https://dzone.com/articles/kafka-producer-delivery-semantics)
- [↑ Kafka Consumer Delivery Semantics](https://dzone.com/articles/kafka-consumer-delivery-semantics)
