# 一、基本信息

主库：

|            名称             | 值                                  |
| :-------------------------: | :---------------------------------- |
|         ORACLE_BASE         | /usr/app/oracle      |
|         ORACLE_HOME         | /usr/app/oracle/product/12/db_1 |
|         ORACLE_SID          | orcl                                |
| DB_UNIQUE_NAME、SERVER_NAME | orcl                             |
|             IP              | 172.21.65.221                       |
|           TNSNAME           | tns_orcl                         |
|       fal_server(TNS)       | tns_orcl_dst                         |
|       fal_client(TNS)       | tns_orcl                         |

备库：

|            名称             | 值                                  |
| :-------------------------: | :---------------------------------- |
|         ORACLE_BASE         | /u01/app/oracle                     |
|         ORACLE_HOME         | /u01/app/oracle/product/11.2.0/db_1 |
|         ORACLE_SID          | orcl                                |
| DB_UNIQUE_NAME、SERVER_NAME | orcl_dst                             |
|             IP              | 172.21.65.233                       |
|           TNSNAME           | tns_orcl_dst                         |
|       fal_server(TNS)       | tns_orcl                         |
|       fal_client(TNS)       | tns_orcl_dst                         |

注：

1. dg中db_name双库相同,在dbca时设置gdbName
2. DB_UNIQUE_NAME在设置pfile时进行确定的。关于db_name和db_unique_name在dg中的区别：[参考文档](http://blog.itpub.net/24742969/viewspace-1614022/)
3. ORACLE_SID可以不相同。
4. fal_server与fal_client在指定的是TNSNAME：[参考文档](https://blog.csdn.net/jiaping0424/article/details/51691386)
5. db_file_name_convert和log_file_name_convert的作用：[参考文档](https://www.cnblogs.com/xqzt/p/5089826.html)
6. 关闭防火墙或允许端口1521

# 二、数据库配置


双库：强制记录日志（务必配置）

   ```sql
   SQL> startup mount
   SQL> alter database force logging;
   SQL> alter database archivelog;
   
   SQL> select log_mode,force_logging from v$database;
   
   LOG_MODE     FOR
   ------------ ---
   ARCHIVELOG   YES
   ```


## 配置主库


1. 设置数据库参数

   db_unique_name：可自定义，==注：该步骤完成后务必重启数据库，以此更新db_unique_name,主库备库同时修改==

   ```sql
   SQL> alter system set db_unique_name='orcl' scope=spfile;
   SQL> shutdown immediate
   SQL> startup
   ```

   dg_config：主库与备库的db_unique_name

   ```sql
   SQL> alter system set log_archive_config='dg_config=(orcl,orcl_dst)' scope=both;
   ```

   location：主库的存档位置(通过`show parameter log_archive_dest_1`查看)、db_unique_name：主库名称

   ```sql
   SQL> alter system set log_archive_dest_1='location=/usr/app/oracle/product/12/db_1/dbs/arch valid_for=(all_logfiles,all_roles) db_unique_name=orcl' scope=both;
   SQL> alter system set log_archive_dest_state_1=enable scope=both;
   ```
   
   service：备库的TNSNAME、db_unique_name：备库名称
   
   ```sql
   SQL> alter system set log_archive_dest_2='service=tns_orcl_dst lgwr async valid_for=(online_logfiles,primary_role) db_unique_name=orcl_dst' scope=both;
   SQL> alter system set log_archive_dest_state_2=enable scope=both;
   ```

   fal_server：备库的TNSNAME、fal_client：主库的TNSNAME
   
   ```sql
   SQL> alter system set fal_server='tns_orcl_dst' scope=both;
   SQL> alter system set fal_client='tns_orcl' scope=both;
   ```

   db_file_name_convent：主库数据文件目录,备库数据文件目录
   
   ```sql
   SQL> alter system set db_file_name_convert='/u01/app/oracle/adg_dst/db_convent','/usr/app/oracle/adg/db_convent' scope=both;
   ```
   
   log_file_name_convert：主库归档日志目录,备库归档日志目录
   
   ```sql
   SQL> alter system set log_file_name_convert='/u01/app/oracle/adg_dst/log_convert','/usr/app/oracle/adg/log_convert' scope=spfile;
   ```
   
   control_file_record_keep_time：修改控制文件保留时间，31天
   
   ```sql
   SQL> alter system set control_file_record_keep_time=31 scope=spfile;
   ```
   
   standby_file_management：自动管理
   
   ```sql
   alter system set standby_file_management=auto scope=both;
   alter system set standby_file_management=manual scope=both;
   ```
   
   确认Standby Logfile
   
   ```sql
   SQL> select group#,bytes from v$standby_log;
   alter database add standby logfile thread 1 group 11 size 50M;
   alter database add standby logfile thread 1 group 12 size 50M;
   alter database add standby logfile thread 1 group 13 size 50M;
   alter database add standby logfile thread 1 group 14 size 50M;
   alter database add standby logfile thread 2 group 21 size 50M;
   alter database add standby logfile thread 2 group 22 size 50M;
   alter database add standby logfile thread 2 group 23 size 50M;
   alter database add standby logfile thread 2 group 24 size 50M;
   
   ```
   
2. 主库再次重启

   ```sql
   SQL> shutdown immediate
   SQL> startup
   # 可发现数据库配置已被更改
   SQL> show parameter name
   ```

4. 主库上创建备库的pfile

   ```sql
   SQL> create pfile='/home/oracle/orcl_dst_pfile' from spfile;
   ```
   
   编辑/home/oracle/orcl_dst_pfile，如果ORALCE_SID不同,修改control_files的路径

   ```ini
   *.db_unique_name='orcl_dst'
   *.audit_file_dest='/u01/app/oracle/admin/orcl_dst/adump'
   *.control_files='/u01/app/oracle/oradata/orcl_dst/control01.ctl','/u01/app/oracle/oradata/orcl_dst/control02.ctl'
   *.log_archive_dest_1='location=/u01/app/oracle/oradata valid_for=(all_logfiles,all_roles) db_unique_name=orcl_dst'
   *.log_archive_dest_2='service=tns_orcl lgwr async valid_for=(online_logfiles,primary_role) db_unique_name=orcl'
   *.fal_server='tns_orcl'
   *.fal_client='tns_orcl_dst'
   ```
   
5. 复制pfile到备库

   ```bash
   scp /home/oracle/orcl_dst_pfile oracle@172.21.65.233:
   ```


## 配置备库

1. 在备库创建audit_file_dest的目录

   ```bash
   [oracle@ora2 ~]$ 
   mkdir -p /u01/app/oracle/admin/orcl_dst/adump
   ```

2. 关闭数据库并创建spfile并修改归档模式、强制归档

   ```sql
   SQL> shutdown immediate
   SQL> startup mount;
   SQL> alter database force logging;
   SQL> alter database archivelog;
   SQL> shutdown immediate
   ```
   
3. 通过pfile启动到nomount模式

   ```sql
   SQL> startup nomount pfile=/home/oracle/orcl_dst_pfile;
   SQL> create spfile from pfile='/home/oracle/orcl_dst_pfile'
   ```

4. 检查参数，能够看到变化的参数

   ```sql
   SQL> show parameter name
   ```

5. 修改数据库角色

   ```sql
   -- mount状态下
   SQL> SELECT database_role FROM v$database;
   DATABASE_ROLE
   ----------------
   PRIMARY
   
   SQL>
   
   Database altered.
   DATABASE_ROLE
   ----------------
   PHYSICAL STANDBY
   
   ```
   
   

## 双库配置tns


1. 修改$ORACLE_HOME/network/admin/tnsnames.ora

   ```ini
   tns_orcl=
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 172.21.65.221)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = orcl)
         (UR=A)
       )
     )
   tns_orcl_dst=  
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 172.21.65.233)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = orcl_dst)
         (UR=A)
       )
     )
   ```

   ==注：SERVICE_NAME是db_unique_name==

2. 测试连接

   ```
   sqlplus sys/Sino_ZJ001@tns_orcl as sysdba
   sqlplus sys/Sino_ZJ001@tns_orcl_dst as sysdba
   ```

   

# 三、初始化数据（主库->备库）

主库执行（主库read write状态，备库nomount状态）：

```bash
[oracle@ora1 admin]$ rman target sys/Sino_ZJ001@tns_orcl auxiliary sys/Sino_ZJ001@tns_orcl_dst
connected to target database: ORACLE (DBID=1956275221)
connected to auxiliary database: ORACLE (not mounted)

rman> DUPLICATE TARGET DATABASE FOR STANDBY FROM ACTIVE DATABASE NOFILENAMECHECK;
# 确定无错误日志后，最后将提示 Finished Duplicate Db at 17-SEP-21，并且双端的select max(sequence#) from v$Log;相同
```

确认同步后，备库开启至open状态

```sql
SQL> alter database recover managed standby database using current logfile disconnect from session;
SQL> alter database recover managed standby database cancel;
SQL> alter database open read only;
SQL> alter database recover managed standby database using current logfile disconnect from session;
```

此时可以插入数据，查看是否同步

## 调试

主库切换日志后，查看SEQUENCE#值，理应相同

```sql
SQL>  alter system switch logfile;
```

双库需相同

```sql
sql > select max(SEQUENCE#) from v$log;
```

查询log

```shell
less +F $ORACLE_BASE/diag/rdbms/<db_name>/$ORALCE_SID/trace/alert_$ORACLE_SID.log
```

