# Redis

[↑ Redis](https://redis.io) is an open source, in-memory data structure store, used as a database, cache, and message broker.

Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams.

Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

Redis is an acronym that stands for **RE**mote **DI**ctionary **S**erver.

## Table of contents

- [Redis](#redis)
  - [Table of contents](#table-of-contents)
  - [Running in Docker](#running-in-docker)
  - [GUI](#gui)
  - [Redis CLI](#redis-cli)
  - [Redis data types](#redis-data-types)
  - [Commands](#commands)
  - [Redis databases](#redis-databases)
  - [.NET library](#net-library)
  - [Distributed locking](#distributed-locking)
  - [Persistence](#persistence)
    - [Redis database persistence](#redis-database-persistence)
    - [Append only file persistence](#append-only-file-persistence)
  - [Repository](#repository)
  - [Links](#links)

## Running in Docker

```bash
docker run --name redis \
--publish 6379:6379 \
redis redis-server \
--appendonly yes
```

## GUI

Try web browser GUI [↑ RedisInsight](https://redislabs.com/redis-enterprise/redis-insight).

## Redis CLI

Use [↑ Redis CLI](https://redis.io/docs/ui/cli) to run Redis commands:

```bash
docker exec -it YOUR_REDIS_CONTAINER_NAME redis-cli
```

## Redis data types

<https://redis.io/docs/data-types>

## Commands

| Command                     | Meaning                                                                          |
| --------------------------- | -------------------------------------------------------------------------------- |
| append KEY VALUE            | Appends a string to the old value. Returns number of characters of the new value |
| decr KEY                    | Decrements value by 1                                                            |
| del KEY                     | Delete key. A key is ignored if it does not exist.                               |
| exec                        | Commits transaction                                                              |
| exists KEY                  | Checks if key exists                                                             |
| flushall                    | Flushes everything                                                               |
| get KEY                     | Gets value by key                                                                |
| getset KEY VALUE            | Gets old value and sets new                                                      |
| hget HASH_NAME KEY          | Gets value inside has by key                                                     |
| hgetall HASH_NAME           | Gets all key/values for the hash                                                 |
| hkeys NAME                  | Gets all keys from hash                                                          |
| hset HASH_NAME KEY VALUE    | Creates hash and sets key and value inside of it                                 |
| hvalues NAME                | Gets all values from hash                                                        |
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

## Redis databases

Out of the box, a Redis instance supports 16 logical databases. These databases are effectively siloed off from one another, and when you run a command in one database, it doesn’t affect any of the data stored in other databases in your Redis instance.

Redis databases are numbered from `0` to `15` and, by default, you connect to database `0` when you connect to your Redis instance. However, you can change the database you’re using with the select command after you connect:

```terminal
127.0.0.1:6379> select 15
```

[↑ How To Manage Redis Databases and Keys](https://www.digitalocean.com/community/cheatsheets/how-to-manage-redis-databases-and-keys).

## .NET library

[↑ StackExchange.Redis](https://stackexchange.github.io/StackExchange.Redis):

```bash
dotnet add package StackExchange.Redis
```

## Distributed locking

Example using [↑ RedLock.net](https://github.com/samcook/RedLock.net) library:

```bash
dotnet add package RedLock.net
```

```csharp
services.AddTransient<IDistributedLockFactory>(serviceProvider =>
{
    var connectionMultiplexer = serviceProvider.GetRequiredService<IConnectionMultiplexer>();
    var redLockMultiplexer = new RedLockMultiplexer(connectionMultiplexer);
    return RedLockFactory.Create(new List<RedLockMultiplexer> { redLockMultiplexer })
});

await using var redLock = await distributedLockFactory.CreateLockAsync(
    resource: "lockResourceName",
    expiryTime: TimeSpan.FromMinutes(1));

if (redLock.IsAcquired)
    DoStuff();
```

## Persistence

[↑ Redis persistence. How Redis writes data to disk](https://redis.io/docs/management/persistence).

[↑ Is Redis Persistence Enabled?](https://stackoverflow.com/questions/28630467/is-redis-persistence-enabled)

### Redis database persistence

Check if is RDB persistence is enabled with `redis-cli`:

```bash
CONFIG GET save
```

If it returns something like that, then it's enabled:

```console
1) "save"
2) "3600 1 300 100 60 10000"
```

If it returns this, then it's disabled:

```bash
1) "save"
2) ""
```

### Append only file persistence

Check is AOF persistence enabled with `redis-cli`:

```bash
CONFIG GET appendonly
```

```console
1) "appendonly"
2) "yes"
```

If you get `yes` — it's enabled, `no` — disabled.

## Repository

<https://github.com/mialkin/redis>.

## Links

- [↑ JSON Web Tokens (JWT) are Dangerous for User Sessions—Here’s a Solution](https://redis.com/blog/json-web-tokens-jwt-are-dangerous-for-user-sessions)
- <https://redis.io/docs/getting-started/faq>
- <https://aws.amazon.com/redis>
