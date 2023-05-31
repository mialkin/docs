# Redis

[↑ Redis](https://redis.io) is an open source, in-memory data structure store, used as a database, cache, and message broker.

Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

## Table of contents

- [Redis](#redis)
  - [Table of contents](#table-of-contents)
  - [Running in Docker](#running-in-docker)
  - [GUI](#gui)
  - [NuGet package](#nuget-package)
  - [Redis CLI](#redis-cli)
  - [Commands](#commands)
  - [Distributed locking](#distributed-locking)
  - [Links](#links)

## Running in Docker

```bash
docker run --name YOUR_REDIS_CONTAINER_NAME \
-d \
-p 6379:6379 \
redis redis-server \
--appendonly yes
```

## GUI

Try web browser GUI [↑ RedisInsight](https://redislabs.com/redis-enterprise/redis-insight).

## NuGet package

[↑ StackExchange.Redis](https://stackexchange.github.io/StackExchange.Redis):

```bash
dotnet add package StackExchange.Redis
```

## Redis CLI

Use [↑ Redis CLI](https://redis.io/docs/ui/cli) to run Redis commands:

```bash
docker exec -it YOUR_REDIS_CONTAINER_NAME redis-cli
```

## Commands

| Command                     | Meaning                                                                          |
| --------------------------- | -------------------------------------------------------------------------------- |
| append KEY VALUE            | Appends a string to the old value. Returns number of characters of the new value |
| decr KEY                    | Decrements value by 1                                                            |
| exec                        | Commits transaction                                                              |
| exists KEY                  | Checks if key exists                                                             |
| flushall                    | Flushes everything                                                               |
| get KEY                     | Gets value by key                                                                |
| getset KEY VALUE            | Gets old value and sets new                                                      |
| hget HASHSET_NAME KEY       | Gets value inside hashset by key                                                 |
| hgetall HASHSET_NAME        | Gets all key/values for the hashset                                              |
| hkeys NAME                  | Gets all keys from hashset                                                       |
| hset HASHSET_NAME KEY VALUE | Creates hashset and sets key and value inside of it                              |
| hvalues NAME                | Gets all values from hashset                                                     |
| incr KEY                    | Increments value by 1                                                            |
| incr counter                | Creates `counter` key and sets its value to 1                                    |
| keys PATTERN                | Returns all the keys satifying certain pattern                                   |
| llen NAME                   | Returns length of the list                                                       |
| lpop NAME                   | Pops first value from the list                                                   |
| lpush NAME VALUE            | Adds value to the beggining of the list                                          |
| lrange NAME 0 1             | Display one element starting from the beggining of the list                      |
| lrange NAME 0 -1            | Display elements starting from the beggining of the list to the end of the list  |
| multi                       | Starts multi commands mode (transaction)                                         |
| publish CHANNEL MESSAGE     | Publish message to the channel                                                   |
| rpop NAME                   | Pops last value from the list                                                    |
| rpush NAME VALUE            | Adds value to the end of the list                                                |
| sadd SET_NAME VALUE         | Creates a set and adds value to it                                               |
| sadd SET_NAME VALUE1 VALUE  | Creates a set and adds to it several values separated by space                   |
| scan 0                      | Gets all available keys                                                          |
| scard NAME                  | Gets cardinality of the set                                                      |
| sdiff NAME1 NAME2           | Gets difference of two sets                                                      |
| set KEY VALUE               | Sets key/value pair                                                              |
| set KEY VALUE ex 20         | Sets value with expiration of 20 seconds                                         |
| set KEY VALUE px 20         | Sets value with expiration of 20 milliseconds                                    |
| sinter NAME1 NAME2          | Gets insersection of two sets                                                    |
| smembers NAME               | Gets all the values from the set                                                 |
| spop NAME                   | Returns and removes a random element of the set                                  |
| subscribe CHANNEL           | Subscries to the channel                                                         |
| sunion NAME1 NAME2          | Gets union of two sets                                                           |
| zadd NAME VALUE             | Add value to ordered set respecting order                                        |
| zrange NAME 0 -1            | Get all values from the ordered set                                              |
| zrange NAME 0 -1 WITHSCORES | Get all values from the ordered set with keys                                    |

## Distributed locking

Example using [↑ RedLock.net](https://github.com/samcook/RedLock.net) library:

```csharp
services.AddTransient<IDistributedLockFactory>(sp =>
{
    var multiplexer = new RedLockMultiplexer(sp.GetRequiredService<IConnectionMultiplexer>());
    return RedLockFactory.Create(new List<RedLockMultiplexer> { multiplexer });
});

using var redLock = await _distributedLockFactory
    .CreateLockAsync("lockResourceName", TimeSpan.FromMinutes(10);

if (redLock.IsAcquired)
    DoStuff();
```

## Links

- [↑ JSON Web Tokens (JWT) are Dangerous for User Sessions—Here’s a Solution](https://redis.com/blog/json-web-tokens-jwt-are-dangerous-for-user-sessions/)
