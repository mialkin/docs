# Terminology

## Replication

**Replication**, also often called **mirroring**, is simply copying all of the data to another location. This allows data to be pulled from two or more locations, which ensures high availability. It can also be quite helpful should the main data location go down for some reason, as the data can still be read from one of the replicas.

## Sharding

**Sharding**, also often called **partitioning**, involves splitting data up based on keys. This increases performance because it reduces the hit on each of the individual resources, allowing them to share the burden rather than having it all in one place.
