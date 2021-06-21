# Redis

- [Redis](#redis)
  - [Installation](#installation)
    - [Docker](#docker)
  - [Commands](#commands)
  - [GUI](#gui)

**Redis** is an open source, in-memory data structure store, used as a database, cache, and message broker.

Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

## Installation

### Docker

```bash
docker run --name CONTAINER_NAME -d -p 6379:6379 --restart=always redis redis-server --appendonly yes
```

## Commands

| Command                     | Meaning                                                                          |
| --------------------------- | -------------------------------------------------------------------------------- |
| set KEY VALUE               | Sets key/value pair                                                              |
| get KEY                     | Gets value by key                                                                |
| exists KEY                  | Checks if key exists                                                             |
| flushall                    | Flushes everything                                                               |
| set KEY VALUE ex 20         | Sets value with expiration of 20 seconds                                         |
| set KEY VALUE px 20         | Sets value with expiration of 20 milliseconds                                    |
| getset KEY VALUE            | Gets old value and sets new                                                      |
| append KEY VALUE            | Appends a string to the old value. Returns number of characters of the new value |
| keys PATTERN                | Returns all the keys satifying certain pattern                                   |
| incr KEY                    | Increments value by 1                                                            |
| incr counter                | Creates `counter` key and sets its value to 1                                    |
| decr KEY                    | Decrements value by 1                                                            |
| hset HASHSET_NAME KEY VALUE | Creates hashset and sets key and value inside of it                              |
| hget HASHSET_NAME KEY       | Gets value inside hashset by key                                                 |
| hgetall HASHSET_NAME        | Gets all key/values for the hashset                                              |
| hkeys NAME                  | Gets all keys from hashset                                                       |
| hvalues NAME                | Gets all values from hashset                                                     |
| sadd SET_NAME VALUE         | Creates a set and adds value to it                                               |
| sadd SET_NAME VALUE1 VALUE  | Creates a set and adds to it several values separated by space                   |
| scan 0                      | Gets all available keys                                                          |
| smembers NAME               | Gets all the values from the set                                                 |
| scard NAME                  | Gets cardinality of the set                                                      |
| sunion NAME1 NAME2          | Gets union of two sets                                                           |
| sdiff NAME1 NAME2           | Gets difference of two sets                                                      |
| sinter NAME1 NAME2          | Gets insersection of two sets                                                    |
| spop NAME                   | Returns and removes a random element of the set                                  |
| lpush NAME VALUE            | Adds value to the beggining of the list                                          |
| lrange NAME 0 1             | Display one element starting from the beggining of the list                      |
| lrange NAME 0 -1            | Display elements starting from the beggining of the list to the end of the list  |
| rpush NAME VALUE            | Adds value to the end of the list                                                |
| lpop NAME                   | Pops first value from the list                                                   |
| rpop NAME                   | Pops last value from the list                                                    |
| llen NAME                   | Returns length of the list                                                       |
| zadd NAME VALUE             | Add value to ordered set respecting order                                        |
| zrange NAME 0 -1            | Get all values from the ordered set                                              |
| zrange NAME 0 -1 WITHSCORES | Get all values from the ordered set with keys                                    |
| multi                       | Starts multi commands mode (transaction)                                         |
| exec                        | Commits transaction                                                              |
| subscribe CHANNEL           | Subscries to the channel                                                         |
| publish CHANNEL MESSAGE     | Publish message to the channel                                                   |

## GUI

Try web browser GUI [↑ RedisInsight](https://redislabs.com/redis-enterprise/redis-insight).
