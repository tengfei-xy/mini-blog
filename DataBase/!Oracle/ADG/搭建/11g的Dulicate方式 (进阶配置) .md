# 一、基本信息

主库：

|            名称             | 值                                  |
| :-------------------------: | :---------------------------------- |
|         ORACLE_BASE         | /u01/app/oracle                     |
|         ORACLE_HOME         | /u01/app/oracle/product/11.2.0/db_1 |
|         ORACLE_SID          | oracle                              |
|           db_name           | ora_dg                              |
| DB_UNIQUE_NAME、SERVER_NAME | uni_prod                            |
|             IP              | 192.168.245.41                      |
|           TNSNAME           | tns_prod                            |
|       fal_server(TNS)       | tns_sbdb                            |
|       fal_client(TNS)       | tns_prod                            |

备库：

|            名称             | 值                                  |
| :-------------------------: | :---------------------------------- |
|         ORACLE_BASE         | /u01/app/oracle                     |
|         ORACLE_HOME         | /u01/app/oracle/product/11.2.0/db_1 |
|         ORACLE_SID          | oracle                              |
|           db_name           | ora_dg                              |
| DB_UNIQUE_NAME、SERVER_NAME | uni_dbsb                            |
|             IP              | 192.168.245.42                      |
|           TNSNAME           | tns_sbdb                            |
|       fal_server(TNS)       | tns_prod                            |
|       fal_client(TNS)       | tns_sbdb                            |

注：

1. dg中db_name双库相同,在dbca时设置gdbName
2. DB_UNIQUE_NAME在设置pfile时进行确定的。关于db_name和db_unique_name在dg中的区别：[参考文档](http://blog.itpub.net/24742969/viewspace-1614022/)
3. ORACLE_SID可以不相同。
4. fal_server与fal_client在指定的是TNSNAME：[参考文档](https://blog.csdn.net/jiaping0424/article/details/51691386)
5. db_file_name_convert和log_file_name_convert的作用：[参考文档](https://www.cnblogs.com/xqzt/p/5089826.html)

# 二、数据库配置

## 配置主库

1. 主库：强制记录日志（务必配置）

   ```sql
   SQL> startup mount
   SQL> alter database force logging;
   SQL> alter database archivelog;
   SQL> alter database open;
   
   SQL> select log_mode,force_logging from v$database;
   
   LOG_MODE     FOR
   ------------ ---
   ARCHIVELOG   YES
   ```

2. 设置数据库参数

   db_unique_name：可自定义，==注：该步骤完成后务必重启数据库，以此更新db_unique_name==

   ```sql
   SQL> alter system set db_unique_name='uni_prod' scope=spfile;
   ```

   dg_config：主库与备库的db_unique_name

   ```sql
   SQL> alter system set log_archive_config='dg_config=(uni_prod,uni_sbdb)' scope=both;
   ```

   location：主库的存档位置(通过archive log list查看)、db_unique_name：主库名称

   ```sql
   SQL> alter system set log_archive_dest_1='location=/u01/app/oracle/product/19.3.0/db_1/dbs/arch valid_for=(all_logfiles,all_roles) db_unique_name=uni_prod' scope=both;
   ```

   service：备库的TNSNAME、db_unique_name：备库名称

   ```sql
   SQL> alter system set log_archive_dest_2='service=uni_sbdb lgwr async valid_for=(online_logfiles,primary_role) db_unique_name=uni_sbdb' scope=both;
   ```

   无需修改，直接执行

   ```sql
   SQL> alter system set log_archive_dest_state_1=enable scope=both;
   SQL> alter system set log_archive_dest_state_2=enable scope=both;
   ```

   fal_server：备库的TNSNAME、fal_client：主库的TNSNAME

   ```sql
   SQL> alter system set fal_server='tns_sbdb' scope=both;
   SQL> alter system set fal_client='tns_prod' scope=both;
   ```

   db_file_name_convent：主库数据文件目录,备库数据文件目录

   ```sql
   SQL> alter system set db_file_name_convert='/u01/app/oracle','/u01/app/oracle' scope=spfile;
   ```

   log_file_name_convent：主库归档日志目录,备库归档日志目录

   ```sql
   SQL> alter system set db_file_name_convert='/u01/app/oracle/product/19.3.0/db_1/dbs/arch','/u01/app/oracle/product/19.3.0/db_1/dbs/arch' scope=spfile;
   ```

3. 主库再次重启

   ```sql
   SQL> shutdown immediate
   SQL> startup
   # 可发现数据库配置已被更改
   SQL> show parameter name
   ```

4. 备库pfile

   主库上创建pfile

   ```sql
   SQL> create pfile='/home/oracle/sbdb_pfile' from spfile;
   ```

   编辑/home/oracle/sbdb_pfile，如果ORALCE_SID不同,修改路径

   ```
   *.audit_file_dest='/u01/app/oracle/admin/ora_dg/adump'
   *.control_files='/u01/app/oracle/oradata/ORA_DG/control01.ctl','/u01/app/oracle/oradata/ORA_DG/control02.ctl'
   ```

   修改db_unique_name

   ```
   *.db_unique_nam='uni_sbdb'
   ```

5. 复制pfile到备库

   ```bash
   scp /home/oracle/sbdb_pfile oracle@192.168.245.42:
   ```




## 配置备库

1. 关闭数据库

   ```sql
   SQL> shutdown immediate
   ```

   ==注：如果sbdb_pfile指定的audit_file_dest不存在的话，记得mkdir==

2. 通过pfile启动到nomount模式

   ```sql
   SQL> startup nomount pfile=/home/oracle/sbdb_pfile;
   ```

3. 检查参数，能够看到变化的参数

   ```sql
   SQL> show parameter name
   ```

4. 创建spfile并修改归档模式（停留在nomount模式）,

   ```sql
   SQL> create spfile from pfile='/home/oracle/sbdb_pfile';
   SQL> alter database force logging;
   SQL> alter database archivelog;
   ```

5. 启动到mount模式,当前模式为PRIMARY

   ```sql
   SQL> startup force mount
   SQL> SELECT database_role FROM v$database;
   DATABASE_ROLE
   ----------------
   PRIMARY
   ```

6. 修改数据库角色

   ```sql
   SQL> ALTER DATABASE CONVERT TO PHYSICAL STANDBY;
   Database altered.
   SQL> SELECT database_role FROM v$database;
   DATABASE_ROLE
   ----------------
   PHYSICAL STANDBY
   ```

   

## 配置tns


1. 双库：修改$ORACLE_SID/network/admin/tnsnames.ora

   ```ini
   tns_prod=
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.245.41)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = uni_prod)
         (UR=A)
       )
     )
   tns_sbdb=  
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.245.42)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = uni_sbdb)
         (UR=A)
       )
     )
   ```

   SERVICE_NAME：最好是db_unique_name

2. 测试连接

   ```
   sqlplus sys/password@tns_prod as sysdba
   sqlplus sys/password@tns_sbdb as sysdba
   ```

   

# 三、初始化数据（主库->备库）

注：转移数据时，双库必须关闭

主库执行：

```bash
[oracle@ora1 admin]$ rman target sys/password@tns_prod auxiliary sys/password@tns_sbdb
connected to target database: ORA_DG (DBID=4286717960)
connected to auxiliary database: ORA_DG (DBID=4286727960)

rman> DUPLICATE TARGET DATABASE FOR STANDBY FROM ACTIVE DATABASE NOFILENAMECHECK;
```

确认同步后，备库开启至open状态

```sql
SQL> alter database recover managed standby database using current logfile disconnect from session;
Database altered.
SQL> alter database recover managed standby database cancel;

```



1. 控制文件

   ```bash
   # 根据备库生成的pfile的*.control_file确定目标位置，其实就是文本最早创建的文件夹的位置
   scp ~/stdby_ctrl_file.bak 192.168.245.42:/u01/app/oracle/oradata/prod/control01.ctl
   scp ~/stdby_ctrl_file.bak 192.168.245.42:/u01/app/oracle/fast_recovery_area/prod/control02.ctl
   ```

2. 密码文件

   ```bash
   scp /u01/app/oracle/product/19.3.0/db_1/dbs/orapworacle 192.168.245.42:/u01/app/oracle/product/19.3.0/db_1/dbs/orapworacle
   ```



## go on（重启数据库后也需要如下操作）

主库

```sql
SQL>  alter system switch logfile;
```

从库

```sql
SQL> recover managed standby database disconnect from session;
-- 此时不能shutdown ,如需要,输入
-- ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
```

此时可以插入数据，查看是否同步

## 调试

双库需相同

```sql
sql > select max(SEQUENCE#) from v$log;
```

查询log

```shell
less +F $ORACLE_BASE/diag/rdbms/<db_name>/$ORALCE_SID/trace/alert_$ORACLE_SID.log
```

