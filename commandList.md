## Redis 键(key) 命令
|命令|描述|
|:---|:---|
|DEL | 该命令用于在 key 存在是删除 key。|
|Dump | 序列化给定 key ，并返回被序列化的值。|
|EXISTS | 检查给定 key 是否存在。|
|Expire | seconds 为给定 key 设置过期时间。|
|Expireat | EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。|
|PEXPIREAT | 设置 key 的过期时间亿以毫秒计。|
|PEXPIREAT | 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计|
|Keys | 查找所有符合给定模式( pattern)的 key 。|
|Move | 将当前数据库的 key 移动到给定的数据库 db 当中。|
|PERSIST | 移除 key 的过期时间，key 将持久保持。|
|Pttl | 以毫秒为单位返回 key 的剩余的过期时间。|
|TTL | 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。|
|RANDOMKEY | 从当前数据库中随机返回一个 key 。|
|Rename | 修改 key 的名称|
|Renamenx | 仅当 newkey 不存在时，将 key 改名为 newkey 。|
|Type | 返回 key 所储存的值的类型。|
## Redis 字符串(String) 命令
|命令|描述|
|:---|:---|
|SET | 设置指定 key 的值|
|Get | 获取指定 key 的值。|
|Getrange | 返回 key 中字符串值的子字符|
|Getset | 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。|
|Getbit | 对 key 所储存的字符串值，获取指定偏移量上的位(bit)。|
|Mget | 获取所有(一个或多个)给定 key 的值。|
|Setbit | 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。|
|Setex | 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。|
|Setnx | 只有在 key 不存在时设置 key 的值。|
|Setrange | 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。|
|Strlen | 返回 key 所储存的字符串值的长度。|
|Mset | 同时设置一个或多个 key-value 对。|
|Msetnx | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。|
|Psetex | 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。|
|Incr | 将 key 中储存的数字值增一。|
|Incrby | 将 key 所储存的值加上给定的增量值（increment） 。|
|Incrbyfloat | 将 key 所储存的值加上给定的浮点增量值（increment） 。|
|Decr | 将 key 中储存的数字值减一。|
|Decrby | key 所储存的值减去给定的减量值（decrement） 。|
|Append | 如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。|
## Redis 哈希(Hash) 命令
|命令|描述|
|:---|:---|
|Hdel | 删除一个或多个哈希表字段|
|Hexists | 查看哈希表 key 中，指定的字段是否存在。|
|Hget | 获取存储在哈希表中指定字段的值/td>|
|Hgetall | 获取在哈希表中指定 key 的所有字段和值|
|Hincrby | 为哈希表 key 中的指定字段的整数值加上增量 increment 。|
|Hincrbyfloat | 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。|
|Hkeys | 获取所有哈希表中的字段|
|Hlen | 获取哈希表中字段的数量|
|Hmget | 获取所有给定字段的值|
|Hmset | 同时将多个 field-value (域-值)对设置到哈希表 key 中。|
|Hset | 将哈希表 key 中的字段 field 的值设为 value 。|
|Hsetnx | 只有在字段 field 不存在时，设置哈希表字段的值。|
|Hvals | 获取哈希表中所有值|
## Redis 列表(List) 命令
|命令|描述|
|:---|:---|
|Blpop | 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|Brpop | 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|Brpoplpush | 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|Lindex | 通过索引获取列表中的元素|
|Linsert | 在列表的元素前或者后插入元素|
|Llen | 获取列表长度|
|Lpop | 移出并获取列表的第一个元素|
|Lpush | 将一个或多个值插入到列表头部|
|Lpushx | 将一个或多个值插入到已存在的列表头部|
|Lrange | 获取列表指定范围内的元素|
|Lrem | 移除列表元素|
|Lset | 通过索引设置列表元素的值|
|Ltrim | 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。|
|Rpop | 移除并获取列表最后一个元素|
|Rpoplpush | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回|
|Rpush | 在列表中添加一个或多个值|
|Rpushx | 为已存在的列表添加值|
## Redis 集合(Set) 命令
|命令|描述|
|:---|:---|
|Sadd | 向集合添加一个或多个成员|
|Scard | 获取集合的成员数|
|Sdiff | 返回给定所有集合的差集|
|Sdiffstore | 返回给定所有集合的差集并存储在 destination 中|
|Sinter | 返回给定所有集合的交集|
|Sinterstore | 返回给定所有集合的交集并存储在 destination 中|
|Sismember | 判断 member 元素是否是集合 key 的成员|
|Smembers | 返回集合中的所有成员|
|Smove | 将 member 元素从 source 集合移动到 destination 集合|
|Spop | 移除并返回集合中的一个随机元素|
|Srandmember | 返回集合中一个或多个随机数|
|Srem | 移除集合中一个或多个成员|
|Sunion | 返回所有给定集合的并集|
|Sunionstore | 所有给定集合的并集存储在 destination 集合中|
|Sscan | 迭代集合中的元素|
## Redis 有序集合(sorted set) 命令
|命令|描述|
|:---|:---|
|Zadd | 向有序集合添加一个或多个成员，或者更新已存在成员的分数|
|Zcard | 获取有序集合的成员数|
|Zcount | 计算在有序集合中指定区间分数的成员数|
|Zincrby | 有序集合中对指定成员的分数加上增量 increment|
|Zinterstore | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中|
|Zlexcount | 在有序集合中计算指定字典区间内成员数量|
|Zrange | 通过索引区间返回有序集合成指定区间内的成员|
|Zrangebylex | 通过字典区间返回有序集合的成员|
|Zrangebyscore | 通过分数返回有序集合指定区间内的成员|
|Zrank | 返回有序集合中指定成员的索引|
|Zrem | 移除有序集合中的一个或多个成员|
|Zremrangebylex | 移除有序集合中给定的字典区间的所有成员|
|Zremrangebyrank | 移除有序集合中给定的排名区间的所有成员|
|Zremrangebyscore | 移除有序集合中给定的分数区间的所有成员|
|Zrevrange | 返回有序集中指定区间内的成员，通过索引，分数从高到底|
|Zrevrangebyscore | 返回有序集中指定分数区间内的成员，分数从高到低排序|
|Zrevrank | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序|
|Zscore | 返回有序集中，成员的分数值|
|Zunionstore | 计算给定的一个或多个有序集的并集，并存储在新的 key 中|
|Zscan | 迭代有序集合中的元素（包括元素成员和元素分值）|
## Redis HyperLogLog 命令
|命令|描述|
|:---|:---|
|Pfadd | 添加指定元素到 HyperLogLog 中。|
|Pfcount | 返回给定 HyperLogLog 的基数估算值。|
|Pgmerge | 将多个 HyperLogLog 合并为一个 HyperLogLog|
## Redis 发布订阅 命令
|命令|描述|
|:---|:---|
|Psubscribe | 订阅一个或多个符合给定模式的频道。|
|Pubsub | 查看订阅与发布系统状态。|
|Publish | 将信息发送到指定的频道。|
|Punsubscribe | 退订所有给定模式的频道。|
|Subscribe | 订阅给定的一个或多个频道的信息。|
|Unsubscribe | 指退订给定的频道。|
## Redis 事务 命令
|命令|描述|
|:---|:---|
|Discard | 取消事务，放弃执行事务块内的所有命令。|
|Exec | 执行所有事务块内的命令。|
|Multi | 标记一个事务块的开始。|
|Unwatch | 取消 WATCH 命令对所有 key 的监视。|
|Watch | 监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。|
## Redis 脚本 命令
|命令|描述|
|:---|:---|
|Eval | 执行 Lua 脚本。|
|Evalsha | 执行 Lua 脚本。|
|Script Exists | 查看指定的脚本是否已经被保存在缓存当中。|
|Script Flush | 从脚本缓存中移除所有脚本。|
|Script kill | 杀死当前正在运行的 Lua 脚本。|
|Script Load | 将脚本 script 添加到脚本缓存中，但并不立即执行这个脚本。|
## Redis 连接 命令
|命令|描述|
|:---|:---|
|Auth | 验证密码是否正确|
|Echo | 打印字符串|
|Ping | 查看服务是否运行|
|Quit | 关闭当前连接|
|Select | 切换到指定的数据库|
## Redis 服务器 命令
|命令|描述|
|:---|:---|
|Bgrewriteaof | 异步执行一个 AOF（AppendOnly File） 文件重写操作|
|Bgsave | 在后台异步保存当前数据库的数据到磁盘|
|Client Kill | 关闭客户端连接|
|Client List | 获取连接到服务器的客户端连接列表|
|Client Getname | 获取连接的名称|
|Client Pause | 在指定时间内终止运行来自客户端的命令|
|Client Setname | 设置当前连接的名称|
|Cluster Slots | 获取集群节点的映射数组|
|Command | 获取 Redis 命令详情数组|
|Command Count | 获取 Redis 命令总数|
|Command Getkeys | 获取给定命令的所有键|
|Time | 返回当前服务器时间|
|Command Info | 获取指定 Redis 命令描述的数组|
|Config Get | 获取指定配置参数的值|
|Config rewrite | 对启动 Redis 服务器时所指定的 redis.conf 配置文件进行改写|
|Config Set | 修改 redis 配置参数，无需重启|
|Config Resetstat | 重置 INFO 命令中的某些统计数据|
|Dbsize | 返回当前数据库的 key 的数量|
|Debug Object | 获取 key 的调试信息|
|Debug Segfault | 让 Redis 服务崩溃|
|Flushall | 删除所有数据库的所有key|
|Flushdb | 删除当前数据库的所有key|
|Info | 获取 Redis 服务器的各种信息和统计数值|
|Lastsave | 返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示|
|Monitor | 实时打印出 Redis 服务器接收到的命令，调试用|
|Role | 返回主从实例所属的角色|
|Save | 异步保存数据到硬盘|
|Shutdown | 异步保存数据到硬盘，并关闭服务器|
|Slaveof | 将当前服务器转变为指定服务器的从属服务器(slave server)|
|Showlog | 管理 redis 的慢日志|
|Sync | 用于复制功能(replication)的内部命令|
