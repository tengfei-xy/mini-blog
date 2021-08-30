## 判断是否安装（只有企业版本有，xe版本没有）

```
SQL> select * from v$option where parameter = 'Oracle Data Guard';

PARAMETER							 VALUE
---------------------------------------------------------------- ----------------------------------------------------------------
Oracle Data Guard						 TRUE
```

## openmode

```
SQL> SELECT open_mode FROM V$DATABASE;
OPEN_MODE
--------------------
READ ONLY WITH APPLY
```

## 服务器角色

```
SQL> SELECT database_role FROM v$database;

DATABASE_ROLE
----------------
PRIMARY
```

## 强制记录日志

```
SQL> select name,force_logging from v$database;

NAME	  FOR
--------- ---
PROD1	  YES

SQL> 
```

如果force_logging是NO

```
SQL> alter database force logging;
```

## 查看自动备份备库文件

```
SQL> show parameter standby_file_management

NAME				     TYPE	 VALUE
------------------------------------ ----------- ------------------------------
standby_file_management 	     string	 AUTO
```

如果VALUE是MANUAL

```
SQL> alter system set standby_file_management='AUTO' scope=both;
```

## 监控 standby 配置是否成功

确认主备库里的归档目的地配置都是有效的

```
sql> select DEST_ID, STATUS, DESTINATION, ERROR from V$ARCHIVE_DEST where DEST_ID<=2;
```

目的地状态 status 应该显示为`VALID`，注意如果上面没有执行 redo 应用会有一条 error 信息 确认重做日志是否真的被应用了，在主库执行

```
sql> select SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED, ARCHIVED from V$ARCHIVED_LOG order by FIRST_TIME;
```

如果归档和日志应用均正常，`APPLIED`和`ARCHIVED`列都应该是 YES。(如果没有应用 redo，applied 应该是 NO)

## 主库上检查是否有重做日志缺口

如果你发现日志没有被应用，那可能是重做日志有了缺口，这种情况下备库无法进行日志应用。但如

果你的`FAL_SERVER`参数设置正确，这应该不会有问题

```
sql> select STATUS, GAP_STATUS from V$ARCHIVE_DEST_STATUS where DEST_ID = 2;
```

如果一切正常，应该返回`VALID`和`NO GAP`.切记启用 redo 应用才能显示`No GAP`在主备库上执行

## 主库上查看日志传送情况

```
sql> select dest_name,error from v$archive_dest where status='ERROR';
```

## 查看dg状态

```
sql> select * from V$DATAGUARD_STATUS order by TIMESTAMP;
```

查看备库状态

```
SELECT PROCESS, STATUS, THREAD#, SEQUENCE#, BLOCK#, DELAY_MINS FROM V$MANAGED_STANDBY;
```

## 查看 MAX(SEQUENCE)

```
sql> select max(SEQUENCE#) from v$log;
```

