# Dulicate方式

## 目录

-   [一、基本信息](#一基本信息)
-   [二、数据库配置](#二数据库配置)
    -   [2.1 配置双库](#21-配置双库)
    -   [2.2 配置主库](#22-配置主库)
    -   [2.3 配置备库](#23-配置备库)
-   [三、初始化数据（主库->备库）](#三初始化数据主库-备库)
    -   [3.1 复制数据文件](#31-复制数据文件)
    -   [3.2 快速测试](#32-快速测试)

# 一、基本信息

| 名称               | 主库            | 备库            |
| ---------------- | ------------- | ------------- |
| ORACLE\_SID      | oracle        | oracle        |
| DB\_UNIQUE\_NAME | ora\_src      | ora\_dst      |
| IP               | 192.168.207.5 | 192.168.207.6 |
| TNSNAME          | tns\_ora\_src | tns\_ora\_dst |

# 二、数据库配置

## 2.1 配置双库

强制记录日志

```sql
SQL> startup mount
SQL> alter database force logging;
SQL> alter database archivelog;

SQL> select log_mode,force_logging from v$database;

LOG_MODE     FOR
------------ ---
ARCHIVELOG   YES
```

修改\$ORACLE\_HOME/network/admin/tnsnames.ora

```ini
tns_ora_src=
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.207.5)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = ora_src)
      (UR=A)
    )
  )
tns_ora_dst=  
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.207.6)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = ora_dst)
      (UR=A)
    )
  )
```

测试连接

```纯文本
sqlplus sys/Password1@tns_ora_src as sysdba
sqlplus sys/Password1@tns_ora_dst as sysdba
```

## 2.2 配置主库

1.  设置数据库参数
    [db\_unique\_name](https://docs.oracle.com/database/121/REFRN/GUID-3547C937-5DDA-49FF-A9F9-14FF306545D8.htm#REFRN10242 "db_unique_name")：数据库唯一名。
    [log\_archive\_dest\_state\_1](https://docs.oracle.com/database/121/REFRN/GUID-983A9C52-3046-4286-AEA7-800741EE0561.htm#REFRN10087 "log_archive_dest_state_1")：默认enable
    [log\_archive\_dest\_state\_2](https://docs.oracle.com/database/121/REFRN/GUID-983A9C52-3046-4286-AEA7-800741EE0561.htm#REFRN10087 "log_archive_dest_state_2")：默认enable
    ```sql
    SQL> alter system set db_unique_name='ora_src' scope=spfile;
    SQL> shutdown immediate
    SQL> startup
    ```
    [log\_archive\_config](https://docs.oracle.com/database/121/REFRN/GUID-4DABDBE9-04B6-44D2-B93D-DAB15EA71427.htm#REFRN10237 "log_archive_config")：启用或禁用将redo日志发送到远程目标和接收远程重做日志，并配置为主库与备库指定唯一的数据库名称(DB\_UNIQUE\_NAME)，12c最多30个并逗号隔开。
    ```sql
    -- alter system set log_archive_config='dg_config=(,,,...)' scope=both;
    SQL> alter system set log_archive_config='dg_config=(ora_src,ora_dst)' scope=both;
    ```
    [log\_archive\_dest\_1](https://docs.oracle.com/database/121/REFRN/GUID-10BD97BF-6295-4E85-A1A3-854E15E05A44.htm#REFRN10086 "log_archive_dest_1")：主库的存档位置(通过`show parameter log_archive_dest_1`查看)、db\_unique\_name：主库名称
    ```sql
    SQL> alter system set log_archive_dest_1='location=/u01/app/oracle/oradata valid_for=(all_logfiles,all_roles) db_unique_name=ora_src' scope=both;
    SQL> alter system set log_archive_dest_state_1=enable scope=both;
    ```
    [log\_archive\_dest\_2](https://docs.oracle.com/database/121/REFRN/GUID-10BD97BF-6295-4E85-A1A3-854E15E05A44.htm#REFRN10086 "log_archive_dest_2")：备库的TNSNAME、db\_unique\_name：备库名称
    ```sql
    SQL> alter system set log_archive_dest_2='service=tns_ora_dst lgwr async valid_for=(online_logfiles,primary_role) db_unique_name=ora_dst' scope=both;
    SQL> alter system set log_archive_dest_state_2=enable scope=both;
    ```
    [fal\_server](https://docs.oracle.com/database/121/REFRN/GUID-4F034B79-AE2A-44E3-8485-E055AA2DDD29.htm#REFRN10056 "fal_server")：备库的TNSNAME、fal\_client：主库的TNSNAME
    ```sql
    SQL> alter system set fal_server='tns_ora_src' scope=both;
    ```
    [db\_file\_name\_convert](https://docs.oracle.com/database/121/REFRN/GUID-E8B4E0EA-B073-4349-9EA9-E053F499FB9E.htm#REFRN10038 "db_file_name_convert")：备库数据文件目录，其次是主库数据文件目录,
    ```sql
    SQL> alter system set db_file_name_convert='/u01/app/oracle','/u01/app/oracle' scope=spfile;
    ```
    [log\_file\_name\_convert](https://docs.oracle.com/database/121/REFRN/GUID-3D5894EF-C33D-4687-978F-F640174F6FCC.htm#REFRN10098 "log_file_name_convert")：备库归档日志目录，其次是主库归档日志目录
    ```sql
    SQL> alter system set log_file_name_convert='/u01/app/oracle/oradata','/u01/app/oracle/oradata' scope=spfile;
    ```
    [standby\_file\_management](https://docs.oracle.com/database/121/REFRN/GUID-BD652D33-31C7-47C9-8019-7A4B79A9D974.htm#REFRN10212 "standby_file_management")：设置为auto可以自动管理standby file文件，设置为manual需要手动删除
    ```sql
    alter system set standby_file_management=auto scope=both;
    -- alter system set standby_file_management=manual scope=both;
    ```
2.  主库重启
    ```sql
    SQL> shutdown immediate
    SQL> startup
    # 可发现数据库配置已被更改
    SQL> show parameter name
    ```
3.  主库上创建备库的pfile
    ```sql
    SQL> create pfile='/home/oracle/ora_dst_pfile' from spfile;
    ```
    编辑/home/oracle/ora\_dst\_pfile，如果ORALCE\_SID不同,修改control\_files等参数的路径
    ```ini
    *.db_unique_name='ora_dst'
    *.control_files='/u01/app/oracle/oradata/oracle/control01.ctl','/u01/app/oracle/flash_recovery_area/oracle/control02.ctl'
    *.log_archive_dest_1='location=/u01/app/oracle/oradata valid_for=(all_logfiles,all_roles) db_unique_name=ora_dst'
    *.log_archive_dest_2='service=tns_ora_src lgwr async valid_for=(online_logfiles,primary_role) db_unique_name=ora_src'
    ```
4.  复制pfile到备库
    ```bash
    scp /home/oracle/ora_dst_pfile oracle@192.168.207.6:
    ```

## 2.3 配置备库

1.  通过pfile启动到nomount模式（注意备份参数文件）
    ```sql
    SQL> startup nomount pfile=/home/oracle/ora_dst_pfile;
    SQL> create spfile from pfile='/home/oracle/ora_dst_pfile';
    ```
2.  检查参数，能够看到变化的参数
    ```sql
    SQL> show parameter name
    ```
3.  修改数据库角色
    ```sql
    -- mount状态下
    SQL> SELECT database_role FROM v$database;
    DATABASE_ROLE
    ----------------
    PRIMARY

    SQL> ALTER DATABASE CONVERT TO PHYSICAL STANDBY;
    Database altered.
    ```

# 三、初始化数据（主库->备库）

## 3.1 复制数据文件

主库执行（主库read write状态，备库nomount状态）

```bash
[oracle@ora1 admin]$ rman target sys/Password1@tns_ora_src auxiliary sys/Password1@tns_ora_dst
connected to target database: ORACLE (DBID=1956275221)
connected to auxiliary database: ORACLE (not mounted)
rman target sys/password@tns_ora_src auxiliary sys/password@tns_ora_dst
rman> DUPLICATE TARGET DATABASE FOR STANDBY FROM ACTIVE DATABASE NOFILENAMECHECK;
```

备库执行

```sql
SQL> select group#,status from v$standby_log;
alter database add standby logfile thread 1 group 1('/u01/app/oracle/oradata/redo1.log') size 200M;
alter database add standby logfile thread 1 group 2('/u01/app/oracle/oradata/redo2.log') size 200M; 
alter database add standby logfile thread 1 group 3('/u01/app/oracle/oradata/redo3.log') size 200M; 
alter database add standby logfile thread 1 group 4('/u01/app/oracle/oradata/redo4.log') size 200M; 
alter database add standby logfile thread 1 group 5('/u01/app/oracle/oradata/redo5.log') size 200M; 
alter database add standby logfile thread 1 group 6('/u01/app/oracle/oradata/redo6.log') size 200M; 
alter database add standby logfile thread 1 group 7('/u01/app/oracle/oradata/redo7.log') size 200M; 
alter database add standby logfile thread 1 group 8('/u01/app/oracle/oradata/redo8.log') size 200M; 
```

实时应用，备库开启至open状态

```sql
SQL> alter database recover managed standby database using current logfile disconnect from session;
SQL> alter database recover managed standby database cancel;
SQL> alter database open read only;
```

此时可以插入数据，查看是否同步

## 3.2 快速测试

主库

```sql
SQL> alter system switch logfile
```

主库、备库查看序列号同步情况

```sql
-- 11g
SQL> select max(sequence#) from v$log;
-- 12c,不要使用archive log list查看(Doc ID 2041137.1)
SQL> select max(sequence#) from v$archived_log;
```

***

参考文档：

<https://mp.weixin.qq.com/s/m1S-ElWOYf_h2kcrre5HNA>

<https://www.modb.pro/db/37720>

<https://docs.oracle.com/database/121/REFRN/GUID-10BD97BF-6295-4E85-A1A3-854E15E05A44.htm#REFRN10086>

<https://www.oracle.com/technetwork/database/availability/active-data-guard-wp-12c-1896127.pdf>

<https://docs.oracle.com/database/121/HABPT/config_dg.htm#HABPT4876>
