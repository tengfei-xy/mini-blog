## 状态确定

主备库状态open和归档模式

```
SQL> select open_mode,log_mode from v$database;

OPEN_MODE	     LOG_MODE
-------------------- ------------
READ WRITE	     ARCHIVELOG
```

主库角色为primery

```sql
SQL> select database_role from v$database;

DATABASE_ROLE
----------------
PRIMARY
```

涉及参数

```sql
show parameter db_file_name_convert
show parameter log_file_name_convert
show parameter standby_file_management
show parameter fal_server
show parameter log_archive_config
show parameter log_archive_dest_2
show parameter log_archive_dest_2_state
```

确定主库归档状态，下方实例为正常状态为

```sql
SQL> select DEST_NAME,STATUS,RECOVERY_MODE from V$ARCHIVE_DEST_STATUS where DEST_NAME='LOG_ARCHIVE_DEST_2';

DEST_NAME
--------------------------------------------------------------------------------
STATUS	  RECOVERY_MODE
--------- -----------------------
LOG_ARCHIVE_DEST_2
VALID	  MANAGED REAL TIME APPLY
```

主库确定备用重做日志是否存在,如没有，需要添加，大小以及文件数量参考备库

```
SQL> select group#,STATUS,ARCHIVED from v$standby_log;
alter database add standby logfile thread 1 group 1('/u01/app/oracle/oradata/redo1.log') size 200M;
alter database add standby logfile thread 1 group 2('/u01/app/oracle/oradata/redo2.log') size 200M; 
alter database add standby logfile thread 1 group 3('/u01/app/oracle/oradata/redo3.log') size 200M; 
```

查看主备库匹配sequence是否统一

```
select max(sequence#) from v$archived_log;
select pid,process,status,sequence# from v$managed_standby;
```

查看备库是否有Archive gap，如有，需要先处理

```bash
select * from V$ARCHIVE_GAP;
```

关闭会话

```sql
-- 查看
select username,sid,status,event,program,machine,sql_id from v$session where username !='SYS';
```

修改log_archive_dest_2

```sql
-- col VALUE_COL_PLUS_SHOW_PARAM for a100
-- show parameter log_archive_dest_2
alter system set log_archive_dest_2='service=tns_ora_src lgwr sync affirm valid_for=(online_logfiles,primary_role) db_unique_name=ora_src'
select dest_name,status,error from v$archive_dest where STATUS='VALID';
```

修改fal_server

```sql
SQL> alter system set fal_server='tns_ora_dst' scope=both;
```

## 进行切换

查看是否能切换角色

```sql
SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
```

主切备

```sql
--  ALTER DATABASE COMMIT TO SWITCHOVER TO PHYSICAL STANDBY;
SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PHYSICAL STANDBY WITH SESSION SHUTDOWN;
SQL> startup nomount;
SQL> alter database mount standby database;
SQL> alter database open read only;
SQL> alter database recover managed standby database using current logfile disconnect from session;
-- alter database recover managed standby database cancel;
```

备切主

```sql
SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO primary;
SQL> select status from v$instance;
SQL> alter database open;
```

