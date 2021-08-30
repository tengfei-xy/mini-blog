sqlplus输入以下内容

```sql
SQL> @ddl_setup.sql
SQL> GRANT GGS_GGSUSER_ROLE TO ogg;
SQL> @ddl_enable.sql
SQL> @ddl_pin.sql ogg;
SQL> GRANT EXECUTE ON utl_file TO ogg;
-- 进入$ORACHE_HOME
SQL> @rdbms/admin/dbmspool.sql;
-- ddl状态查看,其中包含DDL日志路径
SQL> @ddl_status.sql
```

源端extract进程加入以下内容，并重启

```
ddl include all
```

目标端replicate进程加入以下内容，并重启

```
ddl include all 
ddlerror default ignore retryop maxretries 3 retrydelay 5
```

在源数据库追加以下命令后,可从目标端去发现

```
SQL> create index  tengfei.idx_idt1_id2 on tengfei.idt1(id2);
Index created.
```



注：只追加重启后的DDL变换，且日志会更新如下内容

```
DDL : ************************* Start of log for DDL sequence [1], v[ $Id: ddl_setup.sql /st_oggcore_11.2.1/19 2013/07/12 02:27:53 praghuna Exp $ ] trace level [0], owner schema of DDL package [OGG], objtype [INDEX] name [TENGFEI.IDX_IDT1_ID2]
DDLTRACE1 : Before Trigger: point in execution = [1.0], objtype [INDEX] name [TENGFEI.IDX_IDT1_ID2]
DDL : DDL operation [create index  tengfei."IDX_IDT1_ID2" on tengfei."IDT1"(id2)  /* GOLDENGATE_DDL_REPLICATION */ ], sequence [1], DDL type [CREATE] INDEX, real object type [INDEX], validity [VALID], object ID [87411], object [TENGFEI.IDX_IDT1_ID2], real object [TENGFEI.IDT1], base object schema [TENGFEI], base object name [IDT1], logged as [OGG]
DDLTRACE1 : checking create index for parent type:TABLE owner:TENGFEI name:IDT1 object id:87411
DDL : Start SCN found [1222155]
DDL : ------------------------- End of log for DDL sequence [1]
```

