# INDEX

## 目录

-   [INDEX FULL SCAN](#INDEX-FULL-SCAN)
-   [INDEX FAST FULL SCAN](#INDEX-FAST-FULL-SCAN)

## INDEX FULL SCAN

当索引查询包含需要所有访问的数据时，会进行全索引扫描，默认升序。如果索引块没在缓冲区缓存，他会从使用单块从数据文件中读取。要改善这个性能使用索引快速全扫描。

使用方法:`/*+ index(t t_n1) */`、`/*+ index_desc(t t_n1) */`

## INDEX FAST FULL SCAN

使用多块读从数据文件从读取索引块，取回的数据没有索引键存储。

INDEX FULL SCAN比INDEX FAST FULL SCAN在磁盘I/O操作低效，前者只在排序时用到

使用方法:`/*+ index_ffs(t t_n1) */`
