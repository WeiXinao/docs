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




