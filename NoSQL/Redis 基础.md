# 1. NoSQL 数据简介
## 1.1 为什么要引入 NoSQL
1. 保证 session 一致性。

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403081554980.png)

2. 缓解 CPU 和内存压力
3. 减少读 IO 次数

## 1.2 NoSQL 数据库
### 1.2.1 NoSQL 数据库概述
NoSQL (NoSQL = Not only SQL), 意即 「不仅仅是 SQL」，泛指非关系型数据库。NoSQL 不依赖业务逻辑方式存储，而以简单的 key-value 模式存储。因此大大的增加了数据库的扩展能力。
- 不遵循 SQL 标准
- 不支持 ACID
- 远超 SQL 的性能

### 1.2.2 NoSQL 适用场景
- 对数据高并发的读写
- 海量数据的读写
- 数据高可扩展性的

### 1.2.3 NoSQL 不适用场景
- 需要事务支持
- 基于 sql 的结构化查询存储，如复杂的关系，需要**即席**的查询
- **用不着 SQL 和用了 SQL 也不行的情况，请考虑 NoSQL**

### 1.2.4 Memcache
- 很早出现的 NoSQL 数据库 
- 数据都在内存中，一般不持久化
- 支持简单的 key-value 模式。制裁类型单一
- 一般是阻力位缓存数据库辅助持久的数据库。

### 1.2.5 Redis
- 几乎覆盖了 Memcache 的绝大部份功能
- 数据都在内存中，**支持持久化**，主要用作备份恢复
- 除了简单的 key-value 模式，**还支持多种数据结构的存储**，比如 **list、set、hash、zset** 等。
- 一般是作为**缓存数据库**辅助持久化的数据库

### 1.2.6 MongoDB
- 高性能、开源、模式自由（schema free）的**文档型数据库**。
- 数据都在内存中，如果内存不足，把不常用的数据保存到硬盘
- 虽然是 key-value 陌生，但是对 value （尤其是**json**）提供了丰富的查询功能
- 支持二进制数据及大型对象
- 可以根据数据的特点替代 **RDBMS**，成为独立的数据库。或者配合 RDBMS，存储指定的数据。

# 2. Redis 概述安装
- Redis 是一个**开源**的 **key-value** 存储系统。
- 和 Memcached 类似，它支持存储的 value 类型相对更多。包括 **string**（字符串）、**list**（链表）、**set**（集合）、**zset**（sorted set 有序集合）和 **hash** （哈希类型）。
- 这些类型都支持 push/pop、add/remove 及取交集并集和差集及更丰富的操作，而且这些操作都是**原子性**的。
- 在此基础上，Redis 支持各种不同方式的**排序**。
- 与 memcached 一样，为了保证效率，数据都是**缓存在内存**中。
- 区别的是 Redis 会**周期性**的把更新的**数据写入磁盘**或者把修改操作写入追加的
- 并且在此基础上实现了 **master-slave（主从）** 同步

## 2.1 应用场景
### 2.1.1 配合关系型数据库做高速缓存
- 高频次，热门访问的数据，降低数据库 IO
- 分布式架构，做 session 共享

### 2.1.2 多样的数据结构存储持久化数据
 ![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403091500273.png)


### 2.2.3 安装目录：/user/local/bin
查看安装目录：

- redis-benchmark：性能测试工具
- redis-check-aof：修复有问题的 AOF 文件
- redis-check-dump：修复有问题的 dump.rdb 文件
- redis-sentinel：Redis 集群使用
- **redis-server**：Redis 服务器启动命令
- **redis-cli** ：客户端，操作入口

### 2.2.6 Redis 介绍相关知识
端口 **6379** 从何而来？Alessia **Merz**

默认 16 个数据库，类似数组下标从 0 开始，初始**默认值用 0 号库**

- 使用命令 `select <dbid>` 来切换数据库。如：select 8
- 统一密码管理，所有库相同密码。
- **dbsize** 查看当前数据库的 key 的数量
- **flushdb 清空当前库**
- **flushall 通杀全部库**

Redis 是单线程+多路 IO 复用技术

多路复用是指使用一个线程来检查多个文件描述符（Socket）的就绪状态，比如调用 select 和 poll 函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后仅需正在的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）

串行 vs 多线程+锁（memcached） vs 单线程+多路 IO 复用（Redis）

(与 Memcached 三点不同：支持多数据类型，支持持久化，单线程+多路 IO 复用)


![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403081908700.png)

# 3. 常用五大数据类型
哪里去获得 Redis 常见数据类型操作命令：https://redis.io/commands/

## 3.1 Redis 键（Key）
- key \* 查看当前库所有 key （匹配：keys \*1）
- exists key 判断某个 key 是否存在
- type key 查看你的 key 是什么类型
- del key 删除指定的 key 数据
- **unlink 根据 value 选择非阻塞删除** （仅 keys 从 keyspace 元数据中删除，真正删除会在后续异步操作。）
- expire key 10 10 秒钟：为给定的 key 设置过期时间
- ttl key 查看还有多少秒过期，-1 表示永不过期，-2 表示已过期 
- select 命令切换数据库
- dbsize 查看当前数据库的 key 的数量
- flushdb 清空当前库 
- flushall 通杀全部库

## 3.2 Redis 字符串（String）
### 3.2.1 简介
 String 是 Redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value。
 String 类型是**二进制安全的**。意味着 Redis 的 string 可以包含任何数据。比如 jpg 图片或者序列化的对象。
 String 类型是 Redis 最基本的数据类型，一个 Redis 中字符串 value 最多可以是 **512M**

### 3.2.2 常用命令
![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403092126181.png)

\*NX：当数据库中 key 不存在时，可以将 key-value 添加到数据库
\*XX：当数据库中 key 存在时，可以将 key-value 添加数据库，与 NX 参数互斥
\*EX：key 的超时秒数
\*PX：key 的超时毫秒数，与 EX 互斥

| 命令                         | 说明                                      |
| -------------------------- | --------------------------------------- |
| `set <key><value> `        | 添加键值对                                   |
| `get <key>`                | 查询对应键值                                  |
| `append <key><value>`      | 将给定的 `<value>` 追加到原值的末尾                 |
| `strlen <key>`             | 获得值的长度                                  |
| `setnx <key><value>`       | 只有在 key 不存在时，设置 key 的值                  |
| `incr <key>`               | 将 key 中存储的数字值增1<br>只能对数字值操作，如果为空，新增值为1  |
| `decr <key>`               | 将 key 中存储的数字值减1<br>只能对数字值操作，如果为空，新增值为-1 |
| `incrby/decrby <key> <步长>` | 将 key 中存储的数字值增减。自定义步长                   |
原子性 

所谓**原子**操作是指不会被线程调度机制打断的操作。

这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch（切换到另一个线程）。

1. 在单线程中，能够在单条指令中完成的操作都可以认为是“原子操作”，因为终端只能发生于指令之间。
2. 在多线程中，不能被其他进程（线程）打断的操作就叫原子操作。
3. Redis 单命令的原子性主要得益于 Redis 中的单线程。

**案例：**

java 中的 i++ 是否是原子操作？不是

i = 0; 两个线程分别对 i 进行++100 次，值是多少？答案：[\2\-\2\0\0\]\.  在这个范围内都有可能。

解析：

一：200这个值是正常情况下，线程一先执行加到了100，然后线程二加到200.——200.

二：当线程一先加到99，然后被线程二打断，线程二又开始从它的0开始加1，后面又被线程一打断，把线程二的i=1取到，然后又被线程二打断，加到100，又被线程一打断，因为此时线程一为1，前面加到了99，所以只需要再加1就结束，所以最终是2.——2.

最终结果为：【2-200】

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403092214428.png)


| 命令                                          | 说明                                      |
| :------------------------------------------ | :-------------------------------------- |
| `mset <key1><value1><key2><value2>......`   | 同时设置一个或多个 key-value 对                   |
| `mget <key1><key2><key3>......`             | 同时获取一个或多个 value                         |
| `msetnx <key1><value1><key2><value2>......` | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在 |
**原子性：有一个失败则都失败**

| 命令                            | 说明                                                          |
| :---------------------------- | :---------------------------------------------------------- |
| `getrange <key><起始位置><结束位置>`  | 获得值的范围，类似 java 中的 substring, **前包，后包**                      |
| `setrange <key><起始位置><value>` | 用 `<value>` 覆写 `<key>` 所存储的字符串值，从 `<起始位置>` 开始（**索引从 0 开始**） |
| `setex <key><过期时间><value>`    | 设置值的同时，设置过期时间，单位秒                                           |
| `getset <key><value>`         | 以新换旧，设置了新值同时获得旧值                                            |

### 3.2.3 数据结构
String 的数据结构为简单动态字符串（Simple Dynamic String，缩写 SDS）。是可以修改的字符串。内部结构实现上类似于 Java 的 ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403092307004.png)

如图中所示，内部为当前字符串实际分配的空间 capacity，一般要高于实际字符串的长度 len。当字符串长度小于 1M 时，扩容都是加倍现有的空间，如果超过 1M，扩容时一次只会多扩 1M 的空间。需要注意的是字符串最大长度为 512 M。

## 3.3 Redis 列表（List）
### 3.3.1 简介
单键多值

Redis 列表是最简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。

它的底层实际是一个**双向链表**，对两端的操作性能很高，通过索引下标操作中间节点的性能较差。

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403092326810.png)

### 3.3.2 常用命令

| 命令                                             | 说明                                   |
| :--------------------------------------------- | :----------------------------------- |
| `lpush/rpush <key><value1><value2><value3>...` | 从左边/右边插入一个或多个值。                      |
| `lpop/rpop <key>`                              | 从左边/右边吐出一个值。健在值在，值光键亡。               |
| `rpop/lpush <key1><key2>`                      | 从 `<key>` 列表的右边吐出一个值，插到` <key2>` 左边。 |
| `lrange <key><start><stop>`                    | 按照索引下标获得元素（从左到右）                     |
| `lrange mylist 0 -1`                           | 0 左边第一个，**-1 右边第一个，（0~-1表示获取所有）**    |
| `lindex <key><index>`                          | 按照索引下标获得元素（从左到右）                     |
| `llen <key>`                                   | 获得列表长度                               |
| `linsert <key> before <value><newvalue>`       | 在 `<value>` 的后面插入 `<newvalue>` 插入值   |
| `lrem <key><n><value>`                         | 从左边删除 n 个 value （从左到右）               |
| `lset <key><index><value>`                     | 将列表 key 下标为 index 的值替换成 value        |
### 3.3.3 数据结构
List 的数据结构为快速链表 quickList。

首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是 ziplist，也即是压缩列表。

它将所有的元素紧挨着一起存储，分配的是一块连续的内存。

当数据量比较多的时候会改成 quickList。 

因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里存的只是 int 类型的数据，结构上还需要两个额外的的指针 prev 和 next。

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403101834405.png)

Redis 将链表和 ziplist 结合起来组成了 quicklist。也就是将多个 ziplist 使用双向指针串起来使用。这样即满足了快速插入删除性能，又不会出现太大的空间冗余。

## 3.4 Redis 集合（Set）
### 3.4.1 简介
Redis set 对外提供的功能与 list 类似是一个列表的功能，特殊之处在于 set 是可以 **自动排重**的，当你需要存储一个列表数据，又不希望出现重复数据时，set 是一个很好的选择，并且 set 提供了判断某个成员是否在一个 set 集合内的重要接口，这个也是 list 所不能提供的。

Redis 的 Set 是 string 类型的**无序集合。它的底层其实是一个 value 为 null 的 hash 表**，所以添加、删除、查找的**复杂度都是 O(1)**。

一个算法，随着数据的增加，执行时间的长短，如果是 O(1)，数据增加，查找数据的时间不变。

### 3.4.2 常用命令

| 命令                                  | 说明                                              |
| :---------------------------------- | :---------------------------------------------- |
| `add <key><value1><value2>...`      | 将一个或多个 member 元素加入到集合 key 中，已经存在的 member 元素将被忽略 |
| `smembers <key>`                    | 取出该集合中的所有值                                      |
| `sismember <key><value>`            | 判断集合 `<key>` 是否含有该 `<value>` 值，有1，没有 0          |
| `scard <key>`                       | 返回该集合的元素个数                                      |
| `srem <key><value1><value2>...`     | 删除集合中的某个元素。                                     |
| `spop <key>`                        | **随机从该集合中吐出一个值。**                               |
| `srandmember <key><n>`              | 随机从该集合中取出 n 个值。不会从集合中删除。                        |
| `smove <source><destination> value` | 把集合中的值从一个集合移动到另一个集合。                            |
| `sinter <key1><key2>`               | 返回两个集合的**交集**元素                                 |
| `sunion <key1><key2>`               | 返回两个集合的**并集**元素                                 |
| `sdiff <key1><key2>`                | 返回两个集合的**差集**元素（key1中的，不包含 key2 中的）             |
### 3.4.3 数据结构 
Set 数据结构是 dict 字典，字典是用哈希表实现的。

Java 中的 HashSet 的内部实现使用的是 HashMap，只不过所有的 value 都指向同一个对象。Redis 的 set 结构也是一样，它的内部也使用 hash 结构，所有的 value 都指向同一个内部值。

## 3.5 Redis 哈希（Hash）
### 3.5.1 简介
Redis hash 是一个键值对集合。

Redis hash 是一个 string 类型的 **field** 和 **value** 的映射表，hash 特别适合存储对象。类似 Java 里面的 `Map<String,Object>`。

用户 id 为查找的 key，存储的 value 用户对象包含姓名、年龄、生日等信息，如果用普通的 key/value结构来存储。

主要有以下两种方式：

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403102108544.png)

### 3.5.2 常用命令

| 命令                                                | 说明                                              |
| :------------------------------------------------ | :---------------------------------------------- |
| `hset <key><field><value>`                        | 给 `<key>`集合中的 `<field>` 赋值 `<value>`            |
| `hget <key1><field>`                              | 从 `<key1>` 集合 `<field>` 去除 value                |
| `hmset <key1><field1><value1><field2><value2>...` | 批量设置 hash 的值                                    |
| `hexists <key1><field>`                           | 查看哈希表 key 中，给定域 field 是否存在。                     |
| `hkeys <key>`                                     | 列出该 hash 集合中的所有 field                           |
| `hvals <key>`                                     | 列出该 hash 集合中的所有 value                           |
| `hincrby <key><field><increment>`                 | 为哈希表 key 中的域 field 的值加上增量1、-1                   |
| `hsetnx <key><field><value>`                      | 将哈希表 key 中的域 field 的值设置为 value，当且仅当域名 field 不存在 |
### 3.5.3 数据结构
Hash 类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）。当 field-value 长度较短且个数较少时，使用 ziplist，否则使用 hashtable。

## 3.6 Redis 有序集合 Zset（sorted set）
### 3.6.1 简介
Redis 有序集合 zset 与普通集合 set 非常类似，是一个**没有重复元素**的集合。

不同之处是有序集合的每个成员都关联了一个**评分（score）**，这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。**集合中的成员是唯一的，但是评分可以是重复的**
。

因为元素是有序的，所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。

访问有序集合的中间元素也是非常快的，因此你能够使用有序集合作为一个没有重复成员的智能列表。

### 3.6.2 常用命令

| 命令                                                             | 说明                                                                                  |
| :------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| `zadd <key><score><value1><score2><value2>`                    | 将一个或多个 member 元素及其 score 值加入到有序集合 key 当中。                                           |
| `zrange <key><start><stop> [WITHSCORES]`                       | 返回有序集合 key 中，下标在 `<start><stop>` 之间的元素，带 WITHSCORES，可以让分数一起和值返回到结果集。                |
| `zrangebyscore key minmax [withscores] [limit offset count]`   | 返回有序集 key 中，所有 score 介于 min 和 max 之间（包括等于 min 或 max）的成员。有序集成员按 score 值递增（从小到大）次序排列。 |
| `zrevrangebyscore key maxmin [withscore] [limit offset count]` | 同上，改为从大到小排列。                                                                        |
| `zincrby <key><increment><value>`                              | 为元素的 score 加上销量                                                                     |
| `zrem <key><value>`                                            | 删除该集合下，指定值的元素                                                                       |
| `zcount <key><min><max>`                                       | 统计该集合，分区间内的元素个数                                                                     |
| `zrank <key><value>`                                           | 返回该值在集合中的排名，从 0 开始。                                                                 |
### 3.6.3 数据结构
SortedSet(zset) 是 Redis 提供的一个非常特别的数据结构，一方面它等价于 Java 的数据结构  `Map<string, Double>`，可以给每个元素 value 赋予一个权重 score，另一方面它又类似于 TreeSet，内部的元素会按照权重 score 进行排序，可以得到每个元素的名次，还可以通过 score 的范围来获取元素的列表。

zset 底层是用了两个数据结构

1. hash，hash 的作用就是关联元素 value 和权重 score，保证元素 value 的唯一性，可以通过元素 value 找到对应的 score 值。
2. 跳跃表，跳跃表的目的是在于给元素 value 排序，根据 score 的范围获取元素列表。

### 3.6.4 跳跃表（跳表）
1. 简介

	有序集合在生活中比较常见，例如根据成绩对学生排名，根据得分对玩家拍片等，对于有序集合的底层实现，可以使用数组、平衡树、链表等。数组不便元素的插入、删除；平衡树或红黑树虽然效率高但结构复杂；链表查询需要遍历所以效率低。Redis 采用的是跳跃表。跳跃表效率堪比红黑树，实现远比红黑树简单。

2. 实例

	对比有序链表和跳跃表，从链表中查询出 51

	1. 有序链表

		![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403102215178.png)
		需要查找值为 51 的元素，需要从第一个元素开始依次查找、比较才能找到。共需要 6 次比较。

	2. 跳跃表

		![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403102223894.png)
		从第二层开始，1 节点比 51 节点小

		21 节点比 51 节点小，继续向后比较，后面就是 NULL 了，所以从 21 节点向下到第 1 层

		在第 1 层，41 节点比 51 节点小，继续向后，61 节点比 51 节点大，所以从 41 向下

		在第 0 层，51 节点为要查找的节点，节点被找到，共查找 4 次。

		从此可以看出跳跃表比有序链表效率要高

# 4 Redis 配置文件介绍
## 4.1 Units 单位
配置大小写单位，开头定义了一些基本的度量单位，只支持 bytes，不支持 bit

大小写不敏感

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403102235343.png)

## 4.2 INCLUDES 包含
![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403102237923.png)

# 5 Redis 持久化

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152028507.png)


持久化指的是将数据写到持久化的存储，比如一块固态硬盘。Redis 提供了一系列的持久化选项。它们包括：

- **RDB**（Redis Database）：RDB 持久化以特定的时间间隔给你的数据集执行一个时间点的快照。
- **AOF**（Append Only File）：AOF 持久化记录服务器所接受的每一个写操作。这些操作在服务器启动的时候重新执行，重建原数据集。命令以 Redis 协议自身相同的形式被记录。
- **No persistance**：你可以完全禁用持久化。这有时在缓存时使用。
- **RDB + AOF**：你可以在相同的实例中结合AOF 和 RDB。

## 5.1 RDB
在指定的时间间隔，执行数据集的时间点快照。
### 5.1.1 RDB 是如何工作的？

- Redis fork 一个进程。我们现在有了一个子线程和一个父线程。
- 子线程开始将数据集写入临时 RDB 文件。
- 当子线程写完新的 RDB 文件，用它替换旧文件。

这种方法有利于 Redis 写入时复制。

### 5.1.2 优势

1. 适合大规模的数据恢复
2. 按照业务定时备份
3. 对数据完整性和一致性不高
4. RDB 文件在内存中的加载速度要比 AOF  要快

### 5.1.3 劣势

1. 在一定时间间隔做一次备份，所以如果 redis 意外 down 掉的话，就会丢失从当前至最近一次快照间的数据，==快照之间的数据会丢失==
2. 内存数据的全量同步，如果数据量太大会导致 I/O 严重影响服务器性能
3. RDB 依赖于主进程的 fork，在更大的数据集中，这可能会导致服务请求的瞬间延迟。fork 的时候内存中的数据被克隆了一份，大致 2 倍的膨胀性，需要考虑。

### 5.1.4 总结

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152227331.png)

## 5.2 AOF 

**以日志的形式来记录每个写操作**，将 Redis 执行过的所有写指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis 启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复操作。
### 5.2.1 AOF 缓冲区三种写回策略
1. Always

	同步写回，每个写回命令执行完立刻同步地将日志写回磁盘

2. everysec 

	每秒写回，每个命令执行完，只是先写到 AOF 文件的内存缓冲区，每个1秒把缓冲区中的内容写回磁盘

3. no

	操作系统控制的写回，每个命令执行完，只是先把日志写到 AOF 文件的内存缓冲区，由操作系统决定何时将缓冲区内容写回磁盘

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403252225965.png)


### 5.2.2 AOF 重写机制
#### 5.2.2.1 是什么？
![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403252201036.png)

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403252202337.png)

一句话：启动 AOF 文件的内容压缩，只保留可以恢复数据的最小指令集。

#### 5.2.2.2 触发机制
![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403252203188.png)

注意，**同时满足，且的关系**才会触发

1. 根据上次重写后的 aof 大小，判断当前 aof 的大小是不是增长了 1  倍
2. 重写时满足的文件的大小

#### 5.2.2.3 自动触发
满足配置文件中的选项后，Redis 会记录上次重写时的 AOF 大小，默认配置是当 AOF 文件大小是上次 rewrite 后大小的一倍且文件大于 64 M 时
#### 5.2.2.4 手动触发
客户端向服务器发送 `bgwriteaof` 命令
### 5.2.3 重写原理 

**Redis >= 7.0**

- Redis fork 一个子线程，所以现在我们有了一个子线程和一个父线程。
- 子线程开始在一个临时文件中写入一个新的 base AOF
- 父线程打开一个新的 increments AOF 文件去继续写更新。如果重写失败，旧的 base 和 increment 文件（如果存在）加这个新打开的 increment 文件表示完整更新的数据集，所以我们是安全的。
- 当这个子线程完成重写这个 base 文件，父线程收到一个信号，并且使用新打开的增量文件和子线程生成的 base 文件去构建一个 temp manifest，并且持久化它。
- 好处！现在 Redis 做了 manifest 文件的原子替换以便 AOF 重写的结果生效。Redis 也清理旧的 base 文件和任何未使用的增量文件。

**Redis < 7.0**

- Redis fork 一个子线程，所以现在我们有了一个子线程和一个父线程。
- 子线程开始写一个新的 AOF 到一个临时文件。
- 父线程积累所有新的变化到一个内存中的缓存（但是同时他写新的变化到旧的 append-only 文件，所以如果重写失败，我们是安全的）
- 当子线程完成重写文件，父线程收到一个信号，并且追加内存中的缓存到子线程生成的文件的末尾。
- 现在 Redis 原子地重命名新文件到为旧的名字，并且开始开始追加新的数据到新的文件

### 5.2.4 AOF 持久化工作流程

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152212293.png)

1. Client作为命令的来源，会有多个源头以及源源不断的请求命令。
2. 在这些命令到达Redis Server 以后并不是直接写入AOF文件，会将其这些命令先放入AOF缓存中进行保存。这里的AOF缓冲区实际上是内存中的一片区域，存在的目的是当这些命令达到一定量以后再写入磁盘，避免频繁的磁盘IO操作。
3. AOF缓冲会根据AOF缓冲区**同步文件的三种写回策略**将命令写入磁盘上的AOF文件。
4. 随着写入AOF内容的增加为避免文件膨胀，会根据规则进行命令的合并(又称**AOF重写**)，从而起到AOF文件压缩的目的。
5. 当Redis Server 服务器重启的时候会从AOF文件载入数据。

### 5.2.5 优势

1. 更好的保护数据不丢失、性能高、可做紧急恢复

### 5.2.6 劣势

1. 相同数据集而言 aof 文件要远大于 rdb 文件，恢复速度慢于 rdb 
2. aof 运行效率慢于 rdb, 每秒同步策略效率较好，不同步效率和 rdb 相同

### 5.2.7 总结

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152227504.png)

## 5.3 RDB-AOF 混合持久化
![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152321213.png)

RDB 和 AOF 可以共存

### 5.3.1 RDB 和 AOF 共存后听谁的？

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152329725.png)

### 5.3.2 数据恢复顺序和加载流程

在同时开启 rdb 和 aof 持久化时，重启时只会加载 aof 文件，不会加载 rdb 文件

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403152330126.png)

### 5.3.3 怎么选？用哪个？
- RDB 持久化方式能够在指定的时间间隔对你的数据进行快照存储
- AOF 持久化方式记录每次对服务器的写操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF 命令以 redis 协议追加保存每次写的操作到文件末尾。 

### 5.3.4 同时开启两种持久化方式
- 在这种情况下，**当 redis 重启的时候会优先载入 AOF 文件来恢复原始数据**，因为在通常情况下 AOF 文件保存的数据集要比 RDB 文件保存的数据集要完整。
- RDB 的数据不实时，同时使用两者时服务器重启也只会找 AOF 文件。那要不要只使用 AOF 呢？作者建议不要，因为 RDB 更适合用于备份数据库（AOF 在不断变化不好备份），留着 rdb 作为一个万一手段。

### 5.3.5 推荐方式
RDB + AOF 混合方式：结合了RDB和AOF的优点，既能快速加载又能避免丢失过多的数据。

1. 开启混合方式设置
	
	设置aof-use-rdb-preamble的值为 yes   yes表示开启，设置为no表示禁用

2.  RDB+AOF的混合方式

	RDB镜像做全量持久化，AOF做增量持久化

	先使用 RDB 进行快照存储，然后使用 AOF 持久化记录所有的写操作，当重写策略满足或者手动触发重写的时候，将最新的数据存储为新的 RDB 记录。这样的话，重启服务的时候会从 RDB 和 AOF 两部分恢复数据，既保证了数据的完整性，又提高了恢复数据的性能。简单来说：混合持久化方式产生的文件一部分是 RDB 格式，一部分是 AOF 格式。---> **AOF 包括了 RDB 头部 + AOF 混写**

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403252153525.png)

## 5.4 纯缓存模式
同时关闭 RDB + AOF 

```
save ""
```

- 禁用 rdb
- 在禁用 rdb 持久化模式下，我们仍然可以使用命令 `save`、`bgsave` 生成 rdb 文件

```
appendonly no
```

- 禁用 aof 
- 禁用 aof 持久化模式下，我们仍然可以使用命令 `bgrewriteof` 生成 aof 文件。

# 6. Redis 事务
## 6.1 是什么？ 
可以一次执行多个命令，本质是一组命令的集合。一个事务中的所有命令都会序列化，**按顺序地串行化执行而不会被其他命令插入，不许加塞**

## 6.2 能干嘛？
一个队列中，一次性、顺序性、排他性的执行一系列命令

## 6.3 Redis 事务 VS 数据库事务

| 1. 单独的隔离操作 | Redis 的事务仅仅是保证事务里的操作会被连续独占的执行，redis 命令执行是单线程架构，在执行事务内所有指令前是不可能再去同时执行其他客户端的请求的 |
| :--------- | :---------------------------------------------------------------------------- |
| 2. 没有隔离级别  | 因为事务提交前任何指令都不会被实际执行，也就不存在“事务内的查询要看到事务里的更新，事务外的查询不能看到”这种问题了                    |
| 3. 不保证原子性  | Redis 的事务**不保证原子性**，也就是不保证所有指令同时成功或同时失败，只是决定是否有开始执行全部指令的能力，没有执行到一半进行回滚的能力。    |
| 4. 排他性     | Redis 会保证一个事务内的命令依次执行，而不会被其他命令插入。                                             |

## 6.4 总结
- 开启：以 `MULTI` 开启一个事务
- 入队：将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等待执行的事务队列里面
- 执行：由  `EXEC` 命令触发事务

# 7. 管道
## 7.1 定义
是一种消息通信模式：发送者（PUBLISH）发送消息，订阅者（SUBSCRIBE）接收消息，可以实现进程间消息的传递。

一句话：Redis 可以实现消息中间件 MQ 的功能，通过发布订阅实现消息的引导和分流。

==不推荐该功能，专业的事情就交给专业的消息中间件处理，redis 就做好分布式缓存功能==

## 7.2 能干嘛
Redis 客户端可以订阅任意数量的频道，类似我们微信关注多个公众号

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403260919512.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403260921783.png)

发布/订阅其实是一个轻量的队列，只不过数据不会被持久化，一般用来处理**实时性较高的异步消息**

![](https://cdn.jsdelivr.net/gh/WeiXinao/imgBed2@main/img/202403260923775.png)

## 7.3 常用命令 
1. `SUBSCRIBE channel [channel...]`
	- 