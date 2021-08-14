# 一、基本信息

主库：

|    名称     | 值                                  |
| :---------: | :---------------------------------- |
| ORACLE_BASE | /u01/app/oracle                     |
| ORACLE_HOME | /u01/app/oracle/product/11.2.0/db_1 |
| ORACLE_SID  | dg                                  |
|   db_name   | prod                                |
|     IP      | 192.168.3.54                        |

备库：

|    名称     | 值                                  |
| :---------: | :---------------------------------- |
| ORACLE_BASE | /u01/app/oracle                     |
| ORACLE_HOME | /u01/app/oracle/product/11.2.0/db_1 |
| ORACLE_SID  | dg                                  |
|   db_name   | prod                                |
|     IP      | 192.168.3.56                        |

# 二、数据库配置

## 基础设置

1. 主库：强制记录日志

   ```sql
   SQL> start mount
   SQL> alter database force logging;
   SQL> alter database archivelog;
   SQL> alter database open;
   
   SQL> select log_mode,force_logging from v$database;
   
   LOG_MODE     FOR
   ------------ ---
   ARCHIVELOG   YES
   ```

   注：主库务必开启

2. 双库：配置tns

   修该network/admin/tnsnames.ora

   ```ini
   uni_dg1 =
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.3.54)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = uni_dg1)
       )
     )
   uni_dg2 =
     (DESCRIPTION =
       (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.3.56)(PORT = 1521))
       (CONNECT_DATA =
         (SERVER = DEDICATED)
         (SERVICE_NAME = uni_dg2)
       )
     )
   ```

3. 双库：重启监听

   ```
   lsnrctl reload
   ```

4. 双库：建立文件夹

   ```bash
   # pfile 的 LOG_ARCHIVE_DEST_1 的 LOCATION
   mkdir /u01/arch
   # pfile 的 *.control_files 属性
   mkdir -p /u01/app/oracle/fast_recovery_area/prod
   ```

## 配置主库

1. 创建pfile

   ```sql
   SQL> create pfile='/home/oracle/dg_spfile' from spfile;
   SQL> shutdown
   ```

2. 追加以下内容到 /home/oracle/dg_spfile

   ```
   DB_UNIQUE_NAME=uni_dg1
   LOG_ARCHIVE_CONFIG='DG_CONFIG=(uni_dg1,uni_dg2)'
   
   LOG_ARCHIVE_DEST_1=
     'LOCATION=/u01/arch/
     VALID_FOR=(ALL_LOGFILES,ALL_ROLES)
     DB_UNIQUE_NAME=uni_dg1'
   
   LOG_ARCHIVE_DEST_2=
    'SERVICE=uni_dg2 ASYNC
     VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
     DB_UNIQUE_NAME=uni_dg2'
   
   FAL_SERVER=uni_dg2
   DB_FILE_NAME_CONVERT='/u01/app/oracle/oradata/prod','/u01/app/oracle/oradata/prod'
   LOG_FILE_NAME_CONVERT='/u01/app/oracle/oradata/prod','/u01/app/oracle/oradata/prod'
   STANDBY_FILE_MANAGEMENT=AUTO
   LOG_ARCHIVE_DEST_STATE_1=ENABLE
   LOG_ARCHIVE_DEST_STATE_2=ENABLE
   ```

3. 通过pfile启动，注：如果startup出错，在pfile中LOG_ARCHIVE_DEST_1=最后不留空格，下一行留2个空格

   ```sql
   SQL> startup force nomount pfile=/home/oracle/dg_spfile
   ```

4. 查看参数变化

   ```sql
   SQL> show parameter name
   
   NAME				     TYPE	 VALUE
   ------------------------------------ ----------- ------------------------------
   cell_offloadgroup_name		     string
   db_file_name_convert		     string	 /u01/app/oracle/oradata/prod,/u01/app/oracle/oradata/prod
   db_name 			     string	 prod
   db_unique_name			     string	 uni_dg1
   global_names			     boolean	 FALSE
   instance_name			     string	 dg
   lock_name_space 		     string
   log_file_name_convert		     string	 /u01/app/oracle/oradata/prod,/u01/app/oracle/oradata/prod
   processor_group_name		     string
   service_names			     string	 uni_dg1
   ```

   

5. 保存spfile并打开数据库

   ```sql
   SQL> create spfile from pfile='/home/oracle/dg_spfile';
   SQL> startup force
   ```

6. 生成standby的控制文件

   ```sql
   SQL> alter database create standby controlfile as '/home/oracle/stdby_ctrl_file.bak';
   SQL> shutdown
   ```

   

## 转移数据（主库->备库）

注：转移数据时，双库必须关闭

1. 表空间文件和重做日志文件

   ```bash
   scp /u01/app/oracle/oradata/prod/*.log /u01/app/oracle/oradata/prod/*.dbf 192.168.3.56:/u01/app/oracle/oradata/prod
   ```

2. 控制文件

   ```bash
   # 根据备库生成的pfile的*.control_file确定目标位置，其实就是文本最早创建的文件夹的位置
   scp ~/stdby_ctrl_file.bak 192.168.3.56:/u01/app/oracle/oradata/prod/control01.ctl
   scp ~/stdby_ctrl_file.bak 192.168.3.56:/u01/app/oracle/fast_recovery_area/prod/control02.ctl
   ```

3.  密码文件

   ```bash
   scp /u01/app/oracle/product/11.2.0/db_1/dbs/orapwdg 192.168.3.56:/u01/app/oracle/product/11.2.0/db_1/dbs/orapwdgst
   ```

## 配置备库

生成pfile

```sql
SQL> create pfile='/home/oracle/dg_spfile' from spfile;
SQL> shutdown
```

追加以下内容到 /home/oracle/dg_spfile

```
DB_UNIQUE_NAME=uni_dg2
LOG_ARCHIVE_CONFIG='DG_CONFIG=(uni_dg1,uni_dg2)'

LOG_ARCHIVE_DEST_1=
  'LOCATION=/u01/arch/
  VALID_FOR=(ALL_LOGFILES,ALL_ROLES)
  DB_UNIQUE_NAME=uni_dg2'
  
LOG_ARCHIVE_DEST_2=
 'SERVICE=uni_dg1 ASYNC
  VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
  DB_UNIQUE_NAME=uni_dg1'

FAL_SERVER=uni_dg1
DB_FILE_NAME_CONVERT='/u01/app/oracle/oradata/prod','/u01/app/oracle/oradata/prod'
LOG_FILE_NAME_CONVERT='/u01/app/oracle/oradata/prod','/u01/app/oracle/oradata/prod'
STANDBY_FILE_MANAGEMENT=AUTO
LOG_ARCHIVE_DEST_STATE_1=ENABLE
LOG_ARCHIVE_DEST_STATE_2=ENABLE
```

通过pfile启动，注：如果startup出错，在pfile中LOG_ARCHIVE_DEST_1=最后不留空格，下一行留2个空格

```
SQL> startup force nomount pfile=/home/oracle/dg_spfile;
```

检查参数

```
SQL> show parameter name

NAME				     TYPE	 VALUE
------------------------------------ ----------- ------------------------------
cell_offloadgroup_name		     string
db_file_name_convert		     string	 /u01/app/oracle/oradata/prod,/u01/app/oracle/oradata/prod
db_name 			     string	 prod
db_unique_name			     string	 uni_dg2
global_names			     boolean	 FALSE
instance_name			     string	 dg
lock_name_space 		     string
log_file_name_convert		     string	 /u01/app/oracle/oradata/prod,/u01/app/oracle/oradata/prod
processor_group_name		     string
service_names			     string	 uni_dg2

```

创建spfile并启动

```sql
SQL> create spfile from pfile='/home/oracle/dg_spfile';
SQL> startup force
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

