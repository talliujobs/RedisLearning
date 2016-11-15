# Redis 学习笔记

## Redis简介
**Redis优势**  
* 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
* 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
* 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

**Redis与其他key-value存储有什么不同？**  
* Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
* Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。

## Redis 安装
```
sudo apt-get update
sudo apt-get install redis-server
#启动 Redis
redis-server
#查看 redis 是否启动？
redis-cli

```



## Redis 配置
Redis 的配置文件位于 Redis 安装目录下，文件名为 redis.conf。

## Redis 数据类型
Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。
### String（字符串）
string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。
```
redis 127.0.0.1:6379> SET name "runoob"
OK
redis 127.0.0.1:6379> GET name
"runoob"
```
### Hash（哈希）
Redis hash 是一个键值对集合。

Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。
```
127.0.0.1:6379> HMSET user:1 username runoob password runoob points 200
OK
127.0.0.1:6379> HGETALL user:1
1) "username"
2) "runoob"
3) "password"
4) "runoob"
5) "points"
6) "200"
```
### List（列表）
Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
```
redis 127.0.0.1:6379> lpush runoob redis
(integer) 1
redis 127.0.0.1:6379> lpush runoob mongodb
(integer) 2
redis 127.0.0.1:6379> lpush runoob rabitmq
(integer) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
redis 127.0.0.1:6379>
```
### Set（集合）
Redis的Set是string类型的无序集合。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。
**sadd 命令**  
 添加一个string元素到,key对应的set集合中，成功返回1,如果元素已经在集合中返回0,key对应的set不存在返回错误。
```
redis 127.0.0.1:6379> sadd runoob redis
(integer) 1
redis 127.0.0.1:6379> sadd runoob mongodb
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 0
redis 127.0.0.1:6379> smembers runoob

1) "rabitmq"
2) "mongodb"
3) "redis"
```

**zset(sorted set：有序集合)**  
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

**zadd 命令**
添加元素到集合，元素在集合中存在则更新对应score
```
redis 127.0.0.1:6379> zadd runoob 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 0
redis 127.0.0.1:6379> ZRANGEBYSCORE runoob 0 1000

1) "redis"
2) "mongodb"
3) "rabitmq"
```

## Redis 命令
```
#本地
redis-cli
##远程服务器
redis-cli -h host -p port -a password
```

## Redis 键(key)
### Redis keys 命令
|序号	|命令|描述|
|---|:---|:---|
|1|DEL key| 该命令用于在 key 存在时删除 key|
|2|DUMP key| 序列化给定 key ，并返回被序列化的值|
|3|EXISTS key| 检查给定 key 是否存在|
|4|EXPIRE key seconds| 为给定 key 设置过期时间|
|5|EXPIREAT key timestamp |EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)|
|6|PEXPIRE key milliseconds |设置 key 的过期时间以毫秒计|
|7|PEXPIREAT key milliseconds-timestamp| 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计
|8|KEYS pattern| 查找所有符合给定模式( pattern)的 key |
|9|MOVE key db| 将当前数据库的 key 移动到给定的数据库 db 当中|
|10|PERSIST key |移除 key 的过期时间，key 将持久保持|
|11|PTTL key |以毫秒为单位返回 key 的剩余的过期时间|
|12|TTL key |以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)|
|13|RANDOMKEY |从当前数据库中随机返回一个 key |
|14|RENAME key newkey |修改 key 的名称
|15|RENAMENX key newkey |仅当 newkey 不存在时，将 key 改名为 newkey |
|16|TYPE key| 返回 key 所储存的值的类型|

## Redis 字符串(String)
### Redis 字符串命令
|序号|命令|描述|
|---|:---|:---|
|1|SET key value |设置指定 key 的值
|2|GET key |获取指定 key 的值|
|3|GETRANGE key start end |返回 key 中字符串值的子字符|
|4|GETSET key value |将给定 key 的值设为 value ，并返回 key 的旧值(old value)|
|5|GETBIT key offset |对 key 所储存的字符串值，获取指定偏移量上的位(bit)|
|6|MGET key1 [key2..] |获取所有(一个或多个)给定 key 的值|
|7|SETBIT key offset value |对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)|
|8|SETEX key seconds value |将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)|
|9|SETNX key value |只有在 key 不存在时设置 key 的值|
|10|SETRANGE key offset value |用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始|
|11|STRLEN key |返回 key 所储存的字符串值的长度|
|12|MSET key value [key value ...] |同时设置一个或多个 key-value 对|
|13|MSETNX key value [key value ...] |同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在|
|14|PSETEX key milliseconds value |这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位|
|15|INCR key |将 key 中储存的数字值增一|
|16|INCRBY key increment |将 key 所储存的值加上给定的增量值（increment） |
|17|INCRBYFLOAT key increment |将 key 所储存的值加上给定的浮点增量值（increment） |
|18|DECR key |将 key 中储存的数字值减一|
|19|DECRBY key decrement |key 所储存的值减去给定的减量值（decrement） |
|20|APPEND key value |如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾|


## Redis 哈希(Hash)
Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象。
### Redis hash 命令
|序号|命令|描述|
|---|:---|:---|
|1|HDEL key field2 [field2] |删除一个或多个哈希表字段|
|2|HEXISTS key field |查看哈希表 key 中，指定的字段是否存在|
|3|HGET key field |获取存储在哈希表中指定字段的值|
|4|HGETALL key |获取在哈希表中指定 key 的所有字段和值
|5|HINCRBY key field increment |为哈希表 key 中的指定字段的整数值加上增量 increment |
|6|HINCRBYFLOAT key field increment |为哈希表 key 中的指定字段的浮点数值加上增量 increment |
|7|HKEYS key |获取所有哈希表中的字段
|8|HLEN key |获取哈希表中字段的数量
|9|HMGET key field1 [field2] |获取所有给定字段的值
|10|HMSET key field1 value1 [field2 value2 ] |同时将多个 field-value (域-值)对设置到哈希表 key 中|
|11|HSET key field value |将哈希表 key 中的字段 field 的值设为 value |
|12|HSETNX key field value |只有在字段 field 不存在时，设置哈希表字段的值|
|13|HVALS key |获取哈希表中所有值|
|14|HSCAN key cursor [MATCH pattern] [COUNT count] |迭代哈希表中的键值对|

## Redis 列表(List)
 Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）
### Redis 列表命令
|序号|命令|描述|
|---|:---|:---|
|1|BLPOP key1 [key2 ] timeout |移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|2|BRPOP key1 [key2 ] timeout |移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|3|BRPOPLPUSH source destination timeout |从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|4|LINDEX key index |通过索引获取列表中的元素|
|5|LINSERT key BEFORE or AFTER pivot value |在列表的元素前或者后插入元素|
|6|LLEN key |获取列表长度|
|7|LPOP key |移出并获取列表的第一个元素|
|8|LPUSH key value1 [value2] |将一个或多个值插入到列表头部|
|9|LPUSHX key value |将一个或多个值插入到已存在的列表头部|
|10|LRANGE key start stop |获取列表指定范围内的元素|
|11|LREM key count value |移除列表元素|
|12|LSET key index value |通过索引设置列表元素的值|
|13|LTRIM key start stop |对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。|
|14|RPOP key |移除并获取列表最后一个元素|
|15|RPOPLPUSH source destination |移除列表的最后一个元素，并将该元素添加到另一个列表并返回|
|16|RPUSH key value1 [value2] |在列表中添加一个或多个值|
|17|RPUSHX key value |为已存在的列表添加值|


## Redis 集合(Set)
Redis的Set是string类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。
### Redis 集合命令
|序号|命令|描述|
|---|:---|:---|
|1|SADD key member1 [member2] |向集合添加一个或多个成员|
|2|SCARD key |获取集合的成员数|
|3|SDIFF key1 [key2] |返回给定所有集合的差集|
|4|SDIFFSTORE destination key1 [key2] |返回给定所有集合的差集并存储在 destination 中|
|5|SINTER key1 [key2] |返回给定所有集合的交集|
|6|SINTERSTORE destination key1 [key2] |返回给定所有集合的交集并存储在 destination 中|
|7|SISMEMBER key member |判断 member 元素是否是集合 key 的成员|
|8|SMEMBERS key |返回集合中的所有成员|
|9|SMOVE source destination member |将 member 元素从 source 集合移动到 destination 集合|
|10|SPOP key |移除并返回集合中的一个随机元素|
|11|SRANDMEMBER key [count] |返回集合中一个或多个随机数|
|12|SREM key member1 [member2] |移除集合中一个或多个成员|
|13|SUNION key1 [key2] |返回所有给定集合的并集|
|14|SUNIONSTORE destination key1 [key2] |所有给定集合的并集存储在 destination 集合中|
|15|SSCAN key cursor [MATCH pattern] [COUNT count] |迭代集合中的元素|

## Redis 有序集合(sorted set)
 Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的,但分数(score)却可以重复。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

### Redis 有序集合命令
|序号|命令|描述|
|---|:---|:---|
|1|ZADD key score1 member1 [score2 member2] |向有序集合添加一个或多个成员，或者更新已存在成员的分数|
|2|ZCARD key |获取有序集合的成员数|
|3|ZCOUNT key min max |计算在有序集合中指定区间分数的成员数|
|4|ZINCRBY key increment member |有序集合中对指定成员的分数加上增量 increment|
|5|ZINTERSTORE destination numkeys key [key ...] |计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中|
|6|ZLEXCOUNT key min max |在有序集合中计算指定字典区间内成员数量|
|7|ZRANGE key start stop [WITHSCORES] |通过索引区间返回有序集合成指定区间内的成员|
|8|ZRANGEBYLEX key min max [LIMIT offset count] |通过字典区间返回有序集合的成员|
|9|ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT] |通过分数返回有序集合指定区间内的成员|
|10|ZRANK key member |返回有序集合中指定成员的索引|
|11|ZREM key member [member ...] |移除有序集合中的一个或多个成员|
|12|ZREMRANGEBYLEX key min max |移除有序集合中给定的字典区间的所有成员|
|13|ZREMRANGEBYRANK key start stop |移除有序集合中给定的排名区间的所有成员|
|14|ZREMRANGEBYSCORE key min max |移除有序集合中给定的分数区间的所有成员|
|15|ZREVRANGE key start stop [WITHSCORES] |返回有序集中指定区间内的成员，通过索引，分数从高到底|
|16|ZREVRANGEBYSCORE key max min [WITHSCORES] |返回有序集中指定分数区间内的成员，分数从高到低排序|
|17|ZREVRANK key member |返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序|
|18|ZSCORE key member |返回有序集中，成员的分数值|
|19|ZUNIONSTORE destination numkeys key [key ...] |计算给定的一个或多个有序集的并集，并存储在新的 key 中|
|20|ZSCAN key cursor [MATCH pattern] [COUNT count] |迭代有序集合中的元素（包括元素成员和元素分值）|

## Redis HyperLogLog
Redis 在 **2.8.9 **版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

### 什么是基数?
比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。


### Redis HyperLogLog 命令
|序号|命令|描述|
|---|:---|:---|
|1|PFADD key element [element ...] |添加指定元素到 HyperLogLog 中。|
|2|PFCOUNT key [key ...]| 返回给定 HyperLogLog 的基数估算值。|
|3|PFMERGE destkey sourcekey [sourcekey ...]| 将多个 HyperLogLog 合并为一个 HyperLogLog |

## Redis 发布订阅
Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。
Redis 客户端可以订阅任意数量的频道。
### Redis 发布订阅命令
|序号|命令|描述|
|---|:---|:---|
|1|PSUBSCRIBE pattern [pattern ...] |订阅一个或多个符合给定模式的频道|
|2|PUBSUB subcommand [argument [argument ...]] |查看订阅与发布系统状态|
|3|PUBLISH channel message |将信息发送到指定的频道|
|4|PUNSUBSCRIBE [pattern [pattern ...]] |退订所有给定模式的频道|
|5|SUBSCRIBE channel [channel ...] |订阅给定的一个或多个频道的信息|
|6|UNSUBSCRIBE [channel [channel ...]] |指退订给定的频道|

## Redis 事务

Redis 事务可以一次执行多个命令， 并且带有以下两个重要的保证：
* 事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
* 事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。

一个事务从开始到执行会经历以下三个阶段：
* 开始事务。
* 命令入队。
* 执行事务。

### Redis 事务命令
|序号|命令|描述|
|---|:---|:---|
|1|DISCARD |取消事务，放弃执行事务块内的所有命令|
|2|EXEC |执行所有事务块内的命令|
|3|MULTI |标记一个事务块的开始|
|4|UNWATCH |取消 WATCH 命令对所有 key 的监视|
|5|WATCH key [key ...] |监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断|


## Redis 连接
```
redis 127.0.0.1:6379> AUTH "password"
OK
redis 127.0.0.1:6379> PING
PONG
````
### Redis 连接命令

|序号|命令|描述|
|---|:---|:---|
|1|AUTH password|验证密码是否正确|
|2|ECHO message|打印字符串|
|3|PING |查看服务是否运行|
|4|QUIT |关闭当前连接|
|5|SELECT index |切换到指定的数据库|

## Redis 服务器
```
redis 127.0.0.1:6379> INFO

# Server
redis_version:2.8.13
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:c2238b38b1edb0e2
redis_mode:standalone
os:Linux 3.5.0-48-generic x86_64
arch_bits:64
multiplexing_api:epoll
gcc_version:4.7.2
process_id:3856
run_id:0e61abd297771de3fe812a3c21027732ac9f41fe
tcp_port:6379
uptime_in_seconds:11554
uptime_in_days:0
hz:10
lru_clock:16651447
config_file:

# Clients
connected_clients:1
client-longest_output_list:0
client-biggest_input_buf:0
blocked_clients:0

# Memory
used_memory:589016
used_memory_human:575.21K
used_memory_rss:2461696
used_memory_peak:667312
used_memory_peak_human:651.67K
used_memory_lua:33792
mem_fragmentation_ratio:4.18
mem_allocator:jemalloc-3.6.0

# Persistence
loading:0
rdb_changes_since_last_save:3
rdb_bgsave_in_progress:0
rdb_last_save_time:1409158561
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:-1
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok

# Stats
total_connections_received:24
total_commands_processed:294
instantaneous_ops_per_sec:0
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
evicted_keys:0
keyspace_hits:41
keyspace_misses:82
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:264

# Replication
role:master
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:10.49
used_cpu_user:4.96
used_cpu_sys_children:0.00
used_cpu_user_children:0.01

# Keyspace
db0:keys=94,expires=1,avg_ttl=41638810
db1:keys=1,expires=0,avg_ttl=0
db3:keys=1,expires=0,avg_ttl=0
```
### Redis 服务器命令


|序号|命令|描述|
|---|:---|:---|
|1|BGREWRITEAOF |异步执行一个 AOF（AppendOnly File） 文件重写操作|
|2|BGSAVE |在后台异步保存当前数据库的数据到磁盘|
|3|CLIENT KILL [ip:port] [ID client-id] |关闭客户端连接|
|4|CLIENT LIST |获取连接到服务器的客户端连接列表|
|5|CLIENT GETNAME |获取连接的名称|
|6|CLIENT PAUSE timeout |在指定时间内终止运行来自客户端的命令|
|7|CLIENT SETNAME connection-name |设置当前连接的名称|
|8|CLUSTER SLOTS |获取集群节点的映射数组|
|9|COMMAND |获取 Redis 命令详情数组|
|10|COMMAND COUNT |获取 Redis 命令总数|
|11|COMMAND GETKEYS |获取给定命令的所有键|
|12|TIME |返回当前服务器时间|
|13|COMMAND INFO command-name [command-name ...] |获取指定 Redis 命令描述的数组|
|14|CONFIG GET parameter |获取指定配置参数的值|
|15|CONFIG REWRITE |对启动 Redis 服务器时所指定的 redis.conf 配置文件进行改写|
|16|CONFIG SET parameter value |修改 redis 配置参数，无需重启|
|17|CONFIG RESETSTAT |重置 INFO 命令中的某些统计数据|
|18|DBSIZE |返回当前数据库的 key 的数量|
|19|DEBUG OBJECT key |获取 key 的调试信息|
|20|DEBUG SEGFAULT |让 Redis 服务崩溃|
|21|FLUSHALL |删除所有数据库的所有key|
|22|FLUSHDB |删除当前数据库的所有key|
|23|INFO [section] |获取 Redis 服务器的各种信息和统计数值|
|24|LASTSAVE |返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示|
|25|MONITOR |实时打印出 Redis 服务器接收到的命令，调试用|
|26|ROLE |返回主从实例所属的角色|
|27|SAVE |异步保存数据到硬盘|
|28|SHUTDOWN [NOSAVE] [SAVE] |异步保存数据到硬盘，并关闭服务器|
|29|SLAVEOF host port |将当前服务器转变为指定服务器的从属服务器(slave server)|
|30|SLOWLOG subcommand [argument] |管理 redis 的慢日志|
|31|SYNC |用于复制功能(replication)的内部命令|

## Redis 数据备份与恢复
### 备份
```
redis 127.0.0.1:6379> SAVE
OK
```
该命令将在 redis 安装目录中创建dump.rdb文件。

### 恢复数据
 如果需要恢复数据，只需将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。获取 redis 目录可以使用 CONFIG 命令，如下所示：
 ```
 redis 127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/redis/bin"
```
###  Bgsave

创建 redis 备份文件也可以使用命令 BGSAVE，该命令在后台执行。
```
127.0.0.1:6379> BGSAVE
Background saving started
```
## Redis 安全
我们可以通过以下命令查看是否设置了密码验证：
```
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) ""
```
默认情况下 requirepass 参数是空的，这就意味着你无需通过密码验证就可以连接到 redis 服务。

你可以通过以下命令来修改该参数：
```
127.0.0.1:6379> CONFIG set requirepass "runoob"
OK
127.0.0.1:6379> CONFIG get requirepass
1) "requirepass"
2) "runoob"
```
设置密码后，客户端连接 redis 服务就需要密码验证，否则无法执行命令。

AUTH 命令基本语法格式如下：
```
127.0.0.1:6379> AUTH password
```
实例
```
127.0.0.1:6379> AUTH "runoob"
OK
127.0.0.1:6379> SET mykey "Test value"
OK
127.0.0.1:6379> GET mykey
"Test value"
```

## Redis 性能测试
```
redis-benchmark [option] [option value]
```

以下实例同时执行 10000 个请求来检测性能：
```
redis-benchmark -n 10000

PING_INLINE: 141043.72 requests per second
PING_BULK: 142857.14 requests per second
SET: 141442.72 requests per second
GET: 145348.83 requests per second
INCR: 137362.64 requests per second
LPUSH: 145348.83 requests per second
LPOP: 146198.83 requests per second
SADD: 146198.83 requests per second
SPOP: 149253.73 requests per second
LPUSH (needed to benchmark LRANGE): 148588.42 requests per second
LRANGE_100 (first 100 elements): 58411.21 requests per second
LRANGE_300 (first 300 elements): 21195.42 requests per second
LRANGE_500 (first 450 elements): 14539.11 requests per second
LRANGE_600 (first 600 elements): 10504.20 requests per second
MSET (10 keys): 93283.58 requests per second
```

|序号|选项|描述|默认值|
|---|:---|:---|:---|
|1|-h|指定服务器主机名|127.0.0.1|
|2|-p|指定服务器端口|6379|
|3|-s|指定服务器 socket||
|4|-c|指定并发连接数|50|
|5|-n|指定请求数|10000|
|6|-d|以字节的形式指定 SET/GET 值的数据大小|2|
|7|-k|1=keep alive 0=reconnect|1|
|8|-r|SET/GET/INCR 使用随机 key, SADD 使用随机值||
|9|-P|通过管道传输 <numreq> 请求|1|
|10|-q|强制退出 redis。仅显示 query/sec 值||
|11|--csv|以 CSV 格式输出||
|12|-l|生成循环，永久执行测试||
|13|-t|仅运行以逗号分隔的测试命令列表。||
|14|-I|Idle 模式。仅打开 N 个 idle 连接并等待。| |

##Redis 客户端连接

Redis 通过监听一个 TCP 端口或者 Unix socket 的方式来接收来自客户端的连接，当一个连接建立后，Redis 内部会进行以下一些操作：

* 首先，客户端 socket 会被设置为非阻塞模式，因为 Redis 在网络事件处理上采用的是非阻塞多路复用模型。
* 然后为这个 socket 设置 TCP_NODELAY 属性，禁用 Nagle 算法
* 然后创建一个可读的文件事件用于监听这个客户端 socket 的数据发送

### 最大连接数

在 Redis2.4 中，最大连接数是被直接硬编码在代码里面的，而在2.6版本中这个值变成可配置的。

maxclients 的默认值是 10000，你也可以在 redis.conf 中对这个值进行修改。
```
config get maxclients

1) "maxclients"
2) "10000"
```
### 实例

以下实例我们在服务启动时设置最大连接数为 100000：

```
redis-server --maxclients 100000

```
### 客户端命令
|S.N.|命令|描述|
|---|:---|:---|
|1|	CLIENT LIST |返回连接到 redis 服务的客户端列表|
|2|	CLIENT SETNAME|	设置当前连接的名称|
|3|	CLIENT GETNAME|	获取通过 CLIENT SETNAME 命令设置的服务名称|
|4|	CLIENT PAUSE|	挂起客户端连接，指定挂起的时间以毫秒计|
|5|	CLIENT KILL |关闭客户端连接|

##
Redis 管道技术

Redis是一种基于客户端-服务端模型以及请求/响应协议的TCP服务。这意味着通常情况下一个请求会遵循以下步骤：

   * 客户端向服务端发送一个查询请求，并监听Socket返回，通常是以阻塞模式，等待服务端响应。
   * 服务端处理命令，并将结果返回给客户端。

### Redis 管道技术

Redis 管道技术可以在服务端未响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应。
### 实例

查看 redis 管道，只需要启动 redis 实例并输入以下命令：
```
$(echo -en "PING\r\n SET runoobkey redis\r\nGET runoobkey\r\nINCR visitor\r\nINCR visitor\r\nINCR visitor\r\n"; sleep 10) | nc localhost 6379

+PONG
+OK
redis
:1
:2
:3
```
以上实例中我们通过使用 PING 命令查看redis服务是否可用， 之后我们们设置了 runoobkey 的值为 redis，然后我们获取 runoobkey 的值并使得 visitor 自增 3 次。

在返回的结果中我们可以看到这些命令一次性向 redis 服务提交，并最终一次性读取所有服务端的响应
管道技术的优势

管道技术最显著的优势是提高了 redis 服务的性能。
一些测试数据

在下面的测试中，我们将使用Redis的Ruby客户端，支持管道技术特性，测试管道技术对速度的提升效果。
```
require 'rubygems'
require 'redis'
def bench(descr)
start = Time.now
yield
puts "#{descr} #{Time.now-start} seconds"
end
def without_pipelining
r = Redis.new
10000.times {
	r.ping
}
end
def with_pipelining
r = Redis.new
r.pipelined {
	10000.times {
		r.ping
	}
}
end
bench("without pipelining") {
	without_pipelining
}
bench("with pipelining") {
	with_pipelining
}

从处于局域网中的Mac OS X系统上执行上面这个简单脚本的数据表明，开启了管道操作后，往返时延已经被改善得相当低了。

without pipelining 1.185238 seconds
with pipelining 0.250783 seconds
```
如你所见，开启管道后，我们的速度效率提升了5倍。

##
Redis 分区

分区是分割数据到多个Redis实例的处理过程，因此每个实例只保存key的一个子集。
分区的优势

* 通过利用多台计算机内存的和值，允许我们构造更大的数据库。
* 通过多核和多台计算机，允许我们扩展计算能力；通过多台计算机和网络适配器，允许我们扩展网络带宽。

### 分区的不足

redis的一些特性在分区方面表现的不是很好：

* 涉及多个key的操作通常是不被支持的。举例来说，当两个set映射到不同的redis实例上时，你就不能对这两个set执行交集操作。
* 涉及多个key的redis事务不能使用。
* 当使用分区时，数据处理较为复杂，比如你需要处理多个rdb/aof文件，并且从多个实例和主机备份持久化文件。
* 增加或删除容量也比较复杂。redis集群大多数支持在运行时增加、删除节点的透明数据平衡的能力，但是类似于客户端分区、代理等其他系统则不支持这项特性。然而，一种叫做presharding的技术对此是有帮助的。

### 分区类型

Redis 有两种类型分区。 假设有4个Redis实例 R0，R1，R2，R3，和类似user:1，user:2这样的表示用户的多个key，对既定的key有多种不同方式来选择这个key存放在哪个实例中。也就是说，有不同的系统来映射某个key到某个Redis服务。
范围分区

最简单的分区方式是按范围分区，就是映射一定范围的对象到特定的Redis实例。

比如，ID从0到10000的用户会保存到实例R0，ID从10001到 20000的用户会保存到R1，以此类推。

这种方式是可行的，并且在实际中使用，不足就是要有一个区间范围到实例的映射表。这个表要被管理，同时还需要各 种对象的映射表，通常对Redis来说并非是好的方法。
哈希分区

另外一种分区方法是hash分区。这对任何key都适用，也无需是object_name:这种形式，像下面描述的一样简单：

* 用一个hash函数将key转换为一个数字，比如使用crc32 hash函数。对key foobar执行crc32(foobar)会输出类似93024922的整数。
* 对这个整数取模，将其转化为0-3之间的数字，就可以将这个整数映射到4个Redis实例中的一个了。93024922 % 4 = 2，就是说key foobar应该被存到R2实例中。注意：取模操作是取除的余数，通常在多种编程语言中用%操作符实现。


## PHP 使用 Redis

开始在 PHP 中使用 Redis 前， 我们需要确保已经安装了 redis 服务及 PHP redis 驱动，且你的机器上能正常使用 PHP。 接下来让我们安装 PHP redis 驱动：下载地址为:https://github.com/phpredis/phpredis/releases。
### PHP安装redis扩展

以下操作需要在下载的 phpredis 目录中完成：
```
$ wget https://github.com/phpredis/phpredis/archive/2.2.4.tar.gz
$ cd phpredis-2.2.7                      # 进入 phpredis 目录
$ /usr/local/php/bin/phpize              # php安装后的路径
$ ./configure --with-php-config=/usr/local/php/bin/php-config
$ make && make install
```
如果你是 PHP7 版本，则需要下载指定分支：
```
    git clone -b php7 https://github.com/phpredis/phpredis.git
```
### 修改php.ini文件
```
vi /usr/local/php/lib/php.ini
```
增加如下内容:
```
extension_dir = "/usr/local/php/lib/php/extensions/no-debug-zts-20090626"

extension=redis.so
```
安装完成后重启php-fpm 或 apache。查看phpinfo信息，就能看到redis扩展。

### 连接到 redis 服务
```
<?php
    //连接本地的 Redis 服务
   $redis = new Redis();
   $redis->connect('127.0.0.1', 6379);
   echo "Connection to server sucessfully";
         //查看服务是否运行
   echo "Server is running: " . $redis->ping();
?>
```
执行脚本，输出结果为：
```
Connection to server sucessfully
Server is running: PONG
```
### Redis PHP String(字符串) 实例
```
<?php
   //连接本地的 Redis 服务
   $redis = new Redis();
   $redis->connect('127.0.0.1', 6379);
   echo "Connection to server sucessfully";
   //设置 redis 字符串数据
   $redis->set("tutorial-name", "Redis tutorial");
   // 获取存储的数据并输出
   echo "Stored string in redis:: " . $redis->get("tutorial-name");
?>
```
执行脚本，输出结果为：
```
Connection to server sucessfully
Stored string in redis:: Redis tutorial
```
Redis PHP List(列表) 实例
```
<?php
   //连接本地的 Redis 服务
   $redis = new Redis();
   $redis->connect('127.0.0.1', 6379);
   echo "Connection to server sucessfully";
   //存储数据到列表中
   $redis->lpush("tutorial-list", "Redis");
   $redis->lpush("tutorial-list", "Mongodb");
   $redis->lpush("tutorial-list", "Mysql");
   // 获取存储的数据并输出
   $arList = $redis->lrange("tutorial-list", 0 ,5);
   echo "Stored string in redis";
   print_r($arList);
?>
```
执行脚本，输出结果为：
```
Connection to server sucessfully
Stored string in redis
Redis
Mongodb
Mysql
```
### Redis PHP Keys 实例
```
<?php
   //连接本地的 Redis 服务
   $redis = new Redis();
   $redis->connect('127.0.0.1', 6379);
   echo "Connection to server sucessfully";
   // 获取数据并输出
   $arList = $redis->keys("*");
   echo "Stored keys in redis:: ";
   print_r($arList);
?>
```
执行脚本，输出结果为：
```
Connection to server sucessfully
Stored string in redis::
tutorial-name
tutorial-list
```
