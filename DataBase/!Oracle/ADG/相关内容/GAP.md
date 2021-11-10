没有配置FAL_SERVER的手动解决办法

# 物理备份(少量数据文件)

查询V$MANAGED_STANDBY，可以看到MRP的状态是WAIT_FOR_GAP

查看GAP量

```sql
SQL>select * from V$ARCHIVE_GAP;
 THREAD#  LOW_SEQUENCE#  HIGH_SEQUENCE#
----------  -------------  --------------
    1        121958           121967

```

在主库中查询缺失的日志的所在路径和名称

```sql
SQL>SELECT NAME FROM V$ARCHIVED_LOG WHERE THREAD#=1 AND DEST_ID=1 AND SEQUENCE# BETWEEN 121958 AND 121967 ;
```

 将上一步查询出的日志文件由主库拷贝到备库，并在备库注册日志文件

```sql
SQL> alter database register logfile '/other/arc/1_121958_902679684.dbf';
```

# rman scan增量恢复

[参考文档](https://www.modb.pro/db/37751)