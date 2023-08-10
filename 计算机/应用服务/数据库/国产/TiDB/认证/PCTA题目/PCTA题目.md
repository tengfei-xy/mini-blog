# PCTA题目

## 目录

-   [单选](#单选)
-   [多选](#多选)

# 单选

> 下面哪个配置是使用DM进行分表分库合并操作所必须的
> A.全量开启复制
> B.Binlog event filter
> C.Block & Allow Lites
> **D.Table routing**

> 下面那个操作能够检查DM源库（上游）是否符合配置要求
> A.Tiup dmctl --master-addr 172.16.10.71:8261 query-status test
> B.Tiup dmctl --master-addr 172.16.10.71:8261 start-task test
> C.Tiup dmctl --master-addr 172.16.10.71:8261 resume-task test
> **D.Tiup dmctl --master-addr 172.16.10.71:8261 operate-source**
> E.以上都不对

> 对于TiDB Binlog的各个组件，描述正确的为？
> **A.Drainner支持向TiDB，MySQL，Kafka或者本地存储同步**
> B.Pump组件只接受Binlog，但是不会存储，直接传递到Drainer节点
> C.Pump组件中存储的Binlog是无序的
> D.Pump支持TiDB的MySQL两种Binlog格式

> 下面那种备份只能按照热备份进行
> A、SQL语句备份
> B、操作系统拷贝
> C、系统快照
> **D、复制**

> 根据下面条件，当你选择备份策略恢复时，下列最合适的选项为？ - 但宕机时间为零 - 数据量较大（大于1TB）
> A、复制
> B、snapshots（快照）
> **C、BR备份工具**
> D、Mydumper或者dumpling备份工具

> 下列正确描述备份恢复工具BR原理的是
> A、各个节点的备份无法做到一致，需要应用日志，将备份集恢复到一致的时间点
> B、BR工具需要连接到TiDB节点，之后直接对TiKV节点的SST文件备份
> C、当TiKV的leader节点繁忙时，会自动从其他副本进行读取
> **D、BR工具会直接连接到TiKV节点进行SST文件备份**

> 使用TiUP工具部署TIDB数据库，下面哪个要求不是必须的？
> A、如果没有配置节点互信，需要手动指定用户名和密码
> B、在deploy命令中，需要指定数据库版本号
> C、需要指定初始化集群拓扑文件
> **D、TiDb，TiKV和PD节点最少各为3个**

> 使用命令Tiup cluster stop \${cluster-name}来停止TiDB Cluster时，各种节点停止顺序为
> A、PD->TIKV->TiDB->TIFlash
> **B、TiFlash->TiDB-TiKV->PD**
> C、TiKV->TiFlash->TiDB->PD
> D、以上都不对

> 下列MySQL语法TiDB数据库支持的为？
> **A、show grants for**
> B、触发器相关语法
> C、函数相关语法
> D、show slave status

> 下面特性是分布式计算引擎的是
> A、支持MVCC
> B、分布式事务ID的分配，提供全局唯一序列
> **C、负责执行计划的选择**
> D、负责Region的再平衡调度

> 下面那个是关于TiDB的Region的正确描述
> A、当Region的大小低于一定限制的时候，Region的数据会分散到其他Region上
> B、当Region的大小超过一定限制时候（默认144MB），多出限制的数据库会分散到其他较小的Region上
> C、当新加入TiKV节点后，是由于TiDBServer来调度数据平衡
> **D、Region中的leader提供了读写服务**

> TIDB-Server实现的功能有
> A、TIDB数据库的全局授时
> B、负责TiKV节点数据的平衡
> C、进行基于成本的优化
> **D、包含协作处理器进行计算与预处理**

> 下面哪项正确描述了TiDB的下推计算
> **A、TiDB的下推计算由TiDB的Coprocessor组件完成**
> B、下推计算是将数据放到TiDBServer中完成
> C、下推计算机会降低IO带宽需求，对于网络带宽不会降低
> D、不需要在SQL中有任何显式声明会自动评估使用下推计算

> 在OLAP场景中，TiDB-Server会有的问题是什么？
> A、无法做到多表join
> B、并发支持较差
> C、无法使用非结构化数据
> **D、对于大量中间结果的情况，内存消耗较大**

> 下面那种数据库结果支持了列式存储引擎TiFlash的准实时更新？
> A、B+ tree
> **B、LSM tree**
> C、Delta tree
> D、Binary tree

> 下面哪些特点不属于TiSpark节点
> A、对于重量级别的报表和Adhoc操作非常有效
> B、需要在列存储上完成
> **C、支持重量级查询，但是高并发无法支持**
> D、目前可以读取TiKV上的数据

> 哪些是关于TIDB两地三中中心容灾方案特性的错误描述？
> A、PRO可以做到为0
> B、强一致性同步
> **C、支持Paxos协议**
> D、使用了Raft协议

> 下面那个选项，正确描述了TiDB在实现跨IDC的场景中的应用？
> **A、单表可以跨机柜多点写入**
> B、复制是基于Multi-Raft和Paxos两种协议完成的
> C、复制分片无法在跨机柜维度打散
> D、Master节点发生故障后，整个集群需要发生切换

> 哪些选项正确描述了TiDB数据库的集群配置？
> A、存储在TiKV节点中
> **B、修改后必须重启节点生效**
> C、除了持久化到配置文件中
> D、相比系统参数，只有Global和Session作用域

> 下面那个操作修改'test'@'172.16.6.212'的密码为tidb是无效的
> A、使用'test'@'172.16.6.212'用户登录，之后执行 set password=password('tidb');
> B、grant all pprivileges on *.* to 'jack'@‘172.16.6.212’ identified bu 'mysql';
> C、ALTER USER 'test'@172.16.6.212 IDENTIFIED BY 'tidb';
> **D、SET PASSWORD FOR ‘test’@'172.16.6.212'='tidb'**
> D、以上都可以

> 下面哪个关于TiDB数据库各个节点上的文件的描述是错误？
> A、所有的节点都有配置文件，例如PD节点，TiKV节点和TiDB节点
> **B、所有节点都有数据文件，例如PD节点，TiKV节点和TiDB节点**
> C、所有节点都有日志文件，例如PD节点，TiKV节点和TiDB节点
> D、以上都不对

> 下面哪个监控需求，Prometheus+Grafana无法实现
> A、所有SQL查询的耗时等执行信息
> **B、Region的健康状态**
> C、组件及主机运维状态
> D、慢查询SQL语句

> 下面哪个项目可以帮助我们了解到Regions的数量和Region的健康状态
> A、Prometheus + Grafana中的PD项目
> B、Prometheus + Grafana中的Service Port Status项目
> C、Prometheus + Grafana中的TiKV项目
> **D、TiDB DashBoard中的TiKV项目**

> 下面哪个操作在TiDB数据库的不停机版本升级过程中是不做的？
> A、检查TiUP版本、如有必要则升级
> B、使用tiup cluster upgrade 进行升级
> C、检查TiUP Cluster 拓扑配置文件，如有必要必须修改
> **D、执行脚本升级系统数据字典**

> 下列哪些选项需求是TiDBLighting工具无法满足的
> A、能够将CSV文件导入到任何版本的TiDB数据库
> **B、支持读取TiDB Binlog日志，可以进行增量导入**
> C、可以在目标表不为空情况下进行导入
> D、能够直接将Dumpling工具导出的文件，加载进TiKV集群

> 根据条件，选择合适的TiDB Lightniing后端模式？-目标表为空-后端数据库版本为TiDB V5.0.0 - 数据量5T，要求最快速导入
> **A、Local-backend**
> B、Import-backend
> C、TiDB-backend
> D、数据量过大，无法支持

> 关于TiCDC的原理和架构，以下说法正确的有
> A、TiCDC的源端（上游）如果是MySQL数据库， 则Capture读取的是MySQL的Binlog
> B、TiCDC集群中的一个Capture对应一个同步任务
> C、TiCDC可以支持多个同步任务，每个任务的目标端（下游）可以不同
> D、并不是所有的Capture都会对ChangeData排序

> 关于CDC，DM和TIDB Binlog的去呗和共性正确的是哪个？
> A、源数据库（上游）如果是MySQL，可以考虑DM和TIDB Binlog进行同步
> B、TICDC，DM和TIDB Binlog都是依靠Binlog来同步数据的
> C、TiCDC，DM和TiDB Binlog都可以向Kafka同步数据
> **D、三种工具的目标数据（下游）都可以是TiDB数据库**

# 多选

> 30
> A、
> B、
> C、
> D、

> 31
> A、
> B、
> C、
> D、

> 32
> A、
> B、
> C、
> D、

> 33
> A、
> B、
> C、
> D、

> 34A、
> B、
> C、
> D、

> 35
> A、
> B、
> C、
> D、

> 36
> A、
> B、
> C、
> D、

> 37下列那些场景备份恢复工具BR不适用
> **A、只备份某个Schema下面的全部表**
> B、数据量较大并且不允许停机
> C、要求备份集在不进行回复时可以进行验证
> **D、需要备份结果集在MySQL或者MariaDB上进行恢复**
> E、要求支持增量备份

> 38使用BR工具进行单库单表备份恢复，下面正确的有
> A、RateLimit参数可以用来限制单个TiKV执行备份的最大熟虑（单位MiB/s）
> B、BR工具只能备份单库，无法完成单表的备份
> C、数据会被校验，但是未回复的时候每个节点补鞥呢只读自己存储的数据
> D、br backup 命令用户完成备份，用db参数和table指定库名和表名
> E、log-file参数必须制定，否则会影响一致性

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> A、
> B、
> C、
> D、

> 59 关于TiCDC的特性有哪些（选择3项）
> **A、不依靠Binlog实现同步**
> B、支持分表分库的合并操作
> **C、同步的表至少包含一个有效索引（主键或非空唯一索引，且不存在虚拟生成列）****D、如果集群中有多个Capture，则可以实现故障转移**
> E、可以过滤掉指定的操作，比如delte，truncate和drop

> 60在使用TiCDC时，下面正确的操作是？（选择2项）
> A、TiCDC集群无法随着TIDB Cluster部署，只能扩容部署
> **B、在创建同步任务是，changefeed-id 是必须项****C、更新同步任务前，需要暂停任务，之后在恢复任务**
> D、我们可以通过Grafana的TICDC面版进行同步任务的监控
