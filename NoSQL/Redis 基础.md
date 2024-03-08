# NoSQL 数据简介
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

## Redis 概述安装
- Redis 是一个**开源的 key-value 存储系统。
- 和 Memcached 类似，它支持存储的 value 类型相对更多。包括 **string**（字符串）、list（链表）
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

