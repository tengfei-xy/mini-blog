# HASH

## 目录

-   [HASH JOIN](#HASH-JOIN)

## HASH JOIN

概念说明：散列连接是CBO常用的连接方式，优化器用两个表（通常是小表）利用连接键在内存中建立散列表，存储到散列表后，扫描较大的表，同样对JOIN KEY进行HASH后探测散列表，找出与散列表匹配的行。

注：如果表太多，不能一次性写到内存中，就分成分区，写入磁盘缓存段。所以只适合小表。

hint方式：`/*+ USE_HASH(table_name1 table_name2)*/`

使用场景：Hash join在两个表的数据量差别很大的时候。

参考文档：[链接](https://www.cnblogs.com/xqzt/p/4469673.html "链接")
