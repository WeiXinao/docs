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

