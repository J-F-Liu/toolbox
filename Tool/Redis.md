# [Redis](http://redis.io/)

Redis is an open source, BSD licensed, advanced key-value store, often referred to as a NoSQL database.
The essence of a key-value store is the ability to store some data, called a value, inside a key.
This data can later be retrieved only if we know the exact key used to store it.
All Redis keys are strings. Values can be strings, hashes, lists, sets and sorted sets.

In order to achieve its outstanding performance, Redis works with an in-memory dataset.
Depending on your use case, you can persist it either by dumping the dataset to disk every once in a while, or by appending each command to a log.
You can use the "maxmemory" option in the config file to put a limit to the memory Redis can use. If this limit is reached Redis will start to reply with an error to write commands (but will continue to accept read-only commands), or you can configure it to evict keys when the max memory limit is reached in the case you are using Redis for caching.

Redis also supports trivial-to-setup master-slave replication, with very fast non-blocking first synchronization, auto-reconnection on net split and so forth.
Other features include Transactions, Pub/Sub, Lua scripting, Keys with a limited time-to-live, and configuration settings to make Redis behave like a cache.

Redis Strings are binary safe, a String value can be at max 512 Megabytes in length.
Redis Lists are simply lists of strings, sorted by insertion order, implemented as linked list. The max length of a list is 2^32 - 1 elements (4294967295, more than 4 billion of elements per list).
Redis Sets are an unordered collection of Strings.
Redis Hashes are maps between string fields and string values.
Redis Sorted Sets are, similarly to Redis Sets, non repeating collections of Strings. The difference is that every member of a Sorted Set is associated with score, that is used in order to take the sorted set ordered, from the smallest to the greatest score. While members are unique, scores may be repeated.

In Redis, databases are simply identified by a number.
The default DB 0 is for new connections, you can change to a different database with SELECT command.
"config get databases" is the command that lists the available databases, "dbsize" get number of keys in selected db.
In theory Redis can handle up to 2^32 keys, and was tested in practice to handle at least 250 million of keys per instance.
Every list, hash, set, and sorted set, can hold 2^32 elements.

Redis doesn't allow you to query an object's values, values can be arbitrary byte arrays.

## Install
```
pacman -S redis
systemctl enable redis
systemctl start redis
```

## Configure
```
nano /etc/redis.conf
> bind <ip_address> 127.0.0.1
> requirepass <password>
```

## redis-server
```
redis-server /usr/local/etc/redis.conf
redis-server start
redis-server stop
redis-server restart
```

## redis-cli
```
redis-cli -a <password> -n 1
redis-cli shutdown
```

```
// store object in json and keys in hash
set users:9001 "{id: 9001, email: leto@dune.gov, ...}"
hset users:lookup:email leto@dune.gov 9001

// store objects in hash
hset myhash field1 "Hello"
hset myhash field2 "World"

// get all fields
hkeys myhash

// get all fields and values of the hash
hgetall myhash

keys *
type resque:queues

LRANGE resque:failed 0 10
LLEN resque:failed
LPOP resque:failed

// clear list
LTRIM resque:failed 1 0

SMEMBERS resque:queues
LRANGE resque:queue:shell_command 0 10
LRANGE resque:queue:thumbnail 0 10

// Get information and statistics about the server.
info

// Change the selected database for the current connection.
// By default new connections use the database 0.
select <index>

// Delete all the keys of the currently selected DB.
flushdb
```
