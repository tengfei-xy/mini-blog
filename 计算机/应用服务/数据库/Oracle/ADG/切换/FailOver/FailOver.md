# FailOver

Failover操作步骤

停止备库日志应用

```sql
alter database recover managed standby database cancel;
```

关闭备库的日志传输

```sql
alter database recover managed standby database finish force;

--主备之间有gap的情况下，使用上一条语句可能会不成功，则需要执行这条语句
alter database active physical standby database;
```

切换到primary

```sql
alter database commit to switchover to primary with session shutdown;
```

重启数据库

```sql
shutdown immediate
startup
```

查询数据库状态

```sql
select open_mode,database_role from v$database
```
