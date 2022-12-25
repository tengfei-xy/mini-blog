查看数据库角色

```sql
SELECT database_role FROM v$database;
```

查看 SEQUENCE

```sql
select max(SEQUENCE#) from v$log;
select max(SEQUENCE#) from v$archived_log
select name,SEQUENCE# from v$archived_log;
```

备库端检测应用成功的最大日志序列

```sql
select max(sequence#) from v$archived_log where applied = 'YES';
```

查看日志传送情况

```sql
select dest_name,status,error from v$archive_dest;
select dest_name,error from v$archive_dest where status='ERROR';
```

查看备库redo应用状态

```sql
select group#,dbid,status,bytes from v$standby_log;
```

查看GAP

```sql
select * from V$ARCHIVE_GAP;
select STATUS, GAP_STATUS from V$ARCHIVE_DEST_STATUS where DEST_ID = 2;
```

查看归档日志状态

```sql
select thread#,name,STATUS,sequence# from v$archived_log;
```

查看进程状态

```sql
select process,THREAD#,status,sequence# from v$managed_standby;
select pid,process from v$managed_standby;
```

查看保护模式

```sql
select protection_mode,protection_level from v$database;
```

查看重做日志状态

```
select GROUP#,status from v$log;
```

查看备库的延时

```bash
select value from v$dataguard_stats where name='apply lag';
```

备库日志接收日志以及应用日志检查

```
select thread#, sequence#,'Last Applied : ' Logs,to_char(next_time, 'DD-MON-YYYY:HH24:MI:SS') Time from v$archived_log
where sequence# =
(select max(sequence#) from v$archived_log where
 applied = 'YES' )
union select thread#,sequence#,'Last Received : ' Logs,
to_char(next_time, 'DD-MON-YYYY:HH24:MI:SS') Time from v$archived_log
where sequence# = (select max(sequence#) from v$archived_log);
```

备库日志传送以及应用消息

```sql
SELECT MESSAGE,TIMESTAMP FROM V$DATAGUARD_STATUS;
```

