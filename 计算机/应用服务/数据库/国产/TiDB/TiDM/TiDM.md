# TiDM

[TiDB Data Migration（DM）](https://docs.pingcap.com/zh/tidb/stable/dm-overview "TiDB Data Migration（DM）")是一体化的数据迁移任务管理工具，支持从MySQL协议兼容的数据库（MySQL、MariaDB、Aurora MySQL）到TiDB的数据迁移，支持全量的数据库载入和增量的数据库传输，同时可以对表与操作的过滤、分库分表的合并迁移。

方向：从MySQL协议兼容的数据库异步迁移到TiDB

支持：全量和增量

dmctl：用来控制DM集群的命令行工具

DM-master：负责管理和调度数据迁移任务的各项操作

DM-worker：负责执行具体的数据迁移任务，读取上游的Binlog保存到本地并传到TiDB cluster。

使用场景：

1.从MySQL协议兼容的数据库到TiDB

2.源表库与目标库异构表的迁移

3.进行分表分库的合并迁移
