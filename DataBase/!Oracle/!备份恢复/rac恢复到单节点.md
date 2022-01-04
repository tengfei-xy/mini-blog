# 11g rac完全恢复到11g单节点

> 参考文档：
>
> 1. [RAC数据库的RMAN备份异机恢复到单节点数据库](https://www.cnblogs.com/Clark-cloud-database/p/7818484.html)
>2. [open database open resetlogs;](https://dbaclass.com/article/ora-00392-log-7-thread-1-cleared-operation-not-allowed/)
> 3. [RAC数据恢复到单节点](https://blog.csdn.net/a743044559/article/details/77847069)
>4. [RMAN异机恢复实验 RAC+ASM恢复到单实例文件系统_astrisk99的博客-程序员ITS201](https://its201.com/article/astrisk99/73222649)
> 5. [RAC恢复到单机_cjh81378066的博客-程序员ITS401](https://its401.com/article/cjh81378066/100354487)
>5. [通过RMAN恢复11G RAC数据库到单实例](https://open-hl.toutiao.com/a6952833002304963111/?label=related_news&utm_campaign=open&utm_medium=webview&utm_source=o_llq_api&req_id=202106180726220101512070134243E4DD&dt=PCHM10&gy=36dbc44de10b08494af1d70ade43b30e1a746eef438c255b1bf87e689d1e19482795a501b7b9fce3b2795c6b0e898b9abdc57e128735a86c340fe07fc2588640eb817136f29f7bf98440e04db88507d842cfa7fd400fda005885e2432e89d839ad9fb7ccec01549b6537921338fc80fc2c322d53f972df1155a2c791fdc5d21c4fa0ac2fbecf6a72b16d7b5c345bdf4e&crypt=86&a_t=401602792390165839809383527872ff&item_id=6952833002304963111&__docId__=6847121423354298883&__barStyle__=1_1&__statParams__=sourceMedia&__fromId__=__all__&__source__=toutiao&sourceMedia=toutiao&__cmtCnt__=19&__styleType__=4&__publisher_id__=109834868269&openv=&__pf__=detail&fr=normal&from_gid=6916452153943130628&channel_id=88805669586&oppo_anchor=)

> 友情提示：
>
> 1. 开三个终端窗口，一个sqlplus，一个rman，一个shell
> 2. 单节点存储rac的备份文件位置：/home/oracle/rac-backup
> 3. 单节点存储rac恢复后的文件位置：/u01/app/oracle/oradata/restore

## rac备份

rac备份脚本（该脚本rac需要开启归档模式）

> 1. 如果不指定format，请从asmcmd从使用cp复制
>
> ```bash
> # 参考命令:
> # 查看备份文件信息
> RMAN> list backup of database;
> # 复制
> ASMCMD> cp +DATA/demodb/backupset/2021_12_25/nnndf0_full_all_0.265.1092197639 /home/grid
> ```
>
> 2. 如果归档文件缺少且备份时归档压缩，使用`catelog BACKUPPIECE`注册备份片

```sql
RMAN> CONFIGURE CONTROLFILE AUTOBACKUP ON;
-- 同时记录DBID
RMAN> backup as compressed backupset tag full_all database format '/home/oracle/rac-backup/full_%U_%T.bak' PLUS ARCHIVELOG format '/home/oracle/rac-backup/arch_%t_%s.bak';
```

进入asmcmd直接复制到/home/grid

```bash
# 复制参数文件和控制文件（自动备份到一个文件中）
ASMCMD> cp +DATA/demodb/autobackup/2021_12_25/s_1092197886.293.1092197887 /home/grid
# sysaux

# system
ASMCMD> cp  +DATA/demodb/backupset/2021_12_25/nnndf0_full_all_0.262.1092197797 /home/grid
# undotbs1、users、undotbs2
ASMCMD> cp +DATA/demodb/backupset/2021_12_25/nnndf0_full_all_0.261.1092197861 /home/grid
ASMCMD> cp +DATA/demodb/datafile/undotbs1.258.1081074747 /home/grid
```

复制备份文件到单节点(/home/oracle/rac-backup)

## 设置spfile

还原备份的spfile（此时还是二进制的）

```sql
-- 单节点需要open状态
RMAN> restore spfile to '/home/oracle/spfile' from '/home/oracle/rac-backup/s_1092197886.293.1092197887';

Starting restore at 24-DEC-21
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=16 device type=DISK

channel ORA_DISK_1: restoring spfile from AUTOBACKUP /home/oracle/rac-backup/spfile.DEMODB.310himge
channel ORA_DISK_1: SPFILE restore from AUTOBACKUP complete
Finished restore at 24-DEC-21
```

> 如果目标库提示未备份在目标库(mount状态)上执行
>
> ```
> backup database
> CONFIGURE CONTROLFILE AUTOBACKUP ON;
> CONFIGURE CONTROLFILE AUTOBACKUP FORMAT FOR DEVICE TYPE DISK TO '%F'; 
> CONFIGURE CONTROLFILE AUTOBACKUP FORMAT FOR DEVICE TYPE 'SBT_TAPE' TO '%d_%I_%F';
> ```
>

将还原后的spfile转化为pfile

```sql
SQL> create pfile='/home/oracle/pfile' from spfile='/home/oracle/spfile';

File created.
```

## 设置pfile

关闭单节点数据库

```sql
SQL> shut immediate;
Database closed.
Database dismounted.
ORACLE instance shut down.
```



处理pfile文件内容：

1. audit_file_dest, background_dump_dest, core_dump_dest,user_dump_dest, log_archive_dest_1等，与路径有关的参数，修改为你单实例环境的对应路径。
2. 移除cluster_database_instances，cluster_database参数
3. 控制文件路径使用上方查询到的路径

```bash
# 参考命令
grep "^*.*" ./pfile | grep -E "\*.cluster_database|\*.remote_listener|\*.cluster_database_instances|\*.db_recovery_file_dest|\*.audit_trail" -v | sed "s#+DATA#$ORACLE_BASE#g"

# 生成修改后的pfile
# grep "^*.*" ./pfile | grep -E "\*.cluster_database|\*.remote_listener|\*.cluster_database_instances|\*.control_files|\*.audit_trail" -v | sed "s#+DATA#$ORACLE_BASE#g"> pfile.modify

# 修改前pfle
demodb1.__db_cache_size=671088640
demodb2.__db_cache_size=671088640
demodb1.__java_pool_size=16777216
demodb2.__java_pool_size=16777216
demodb1.__large_pool_size=33554432
demodb2.__large_pool_size=33554432
demodb1.__oracle_base='/u01/app/oracle'#ORACLE_BASE set from environment
demodb2.__oracle_base='/u01/app/oracle'#ORACLE_BASE set from environment
demodb1.__pga_aggregate_target=553648128
demodb2.__pga_aggregate_target=553648128
demodb1.__sga_target=1040187392
demodb2.__sga_target=1040187392
demodb1.__shared_io_pool_size=0
demodb2.__shared_io_pool_size=0
demodb1.__shared_pool_size=301989888
demodb2.__shared_pool_size=301989888
demodb1.__streams_pool_size=0
demodb2.__streams_pool_size=0
*.audit_file_dest='/u01/app/oracle/admin/demodb/adump'
*.audit_trail='db'
*.cluster_database=true
*.compatible='11.2.0.4.0'
*.control_files='+DATA/demodb/controlfile/current.267.1081074889','+DATA/demodb/controlfile/current.256.1081074889'
*.db_block_size=8192
*.db_create_file_dest='+DATA'
*.db_domain=''
*.db_name='demodb'
*.db_recovery_file_dest='+DATA'
*.db_recovery_file_dest_size=4621074432
*.diagnostic_dest='/u01/app/oracle'
*.dispatchers='(PROTOCOL=TCP) (SERVICE=demodbXDB)'
demodb1.instance_number=1
demodb2.instance_number=2
*.memory_target=1588592640
*.open_cursors=300
*.processes=150
*.remote_listener='ora-scan:1521'
*.remote_login_passwordfile='exclusive'
demodb2.thread=2
demodb1.thread=1
demodb1.undo_tablespace='UNDOTBS1'
demodb2.undo_tablespace='UNDOTBS2'


# 修改后的pfile.modify
*.audit_file_dest='/u01/app/oracle/admin/demodb/adump'
*.compatible='11.2.0.4.0'
*.db_block_size=8192
*.db_create_file_dest='/u01/app/oracle'
*.db_domain=''
*.db_name='demodb'
*.db_recovery_file_dest='/u01/app/oracle'
*.db_recovery_file_dest_size=4621074432
*.diagnostic_dest='/u01/app/oracle'
*.dispatchers='(PROTOCOL=TCP) (SERVICE=demodbXDB)'
*.log_archive_dest_1='location=/u01/app/oracle/archivelog'
*.memory_target=1588592640
*.open_cursors=300
*.processes=150
*.remote_login_passwordfile='exclusive'
```

根据pfile.modify创建相关文件夹

```bash
mkdir -p /u01/app/oracle/admin/demodb/adump
mkdir -p /u01/app/oracle/archivelog
mkdir -p /u01/app/oracle/oradata/restore
```

启动（注意备份spfile）

```sql
SQL> startup nomount pfile='/home/oracle/pfile.modify'
```

保存参数文件

```sql
SQL> create spfile from pfile='/home/oracle/pfile.modify';
```

## 设置控制文件

获取DBID并删除日志文件

```bash
[oracle@localhost ~]$ grep "DBID=[[:digit:]]*" /home/oracle/rac-backup/rman.log -o && rm /home/oracle/rac-backup/rman.log
DBID=4077342536
```

恢复控制文件

```sql
[oracle@localhost ~]$ rman target /
Recovery Manager: Release 11.2.0.4.0 - Production on Fri Dec 24 20:35:59 2021
Copyright (c) 1982, 2011, Oracle and/or its affiliates.  All rights reserved.
connected to target database: DEMODB (not mounted)

RMAN> set dbid=4077342536;
executing command: SET DBID

RMAN> restore controlfile from '/home/oracle/rac-backup/s_1092197886.293.1092197887';

Starting restore at 25-DEC-21
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=10 device type=DISK

channel ORA_DISK_1: restoring control file
channel ORA_DISK_1: restore complete, elapsed time: 00:00:01
output file name=/u01/app/oracle/DEMODB/controlfile/o1_mf_jwdjhswt_.ctl
output file name=/u01/app/oracle/DEMODB/controlfile/o1_mf_jwdjhsxg_.ctl
Finished restore at 25-DEC-21
```

启动到mount

```sql
RMAN>  alter database mount;
database mounted
released channel: ORA_DISK_1
```

加载备份文件

```sql
-- 因为备份集更改了位置，所以删除控制文件中记录的旧信息，注册现在的备份集
RMAN> crosscheck backup of database;
RMAN> crosscheck backup of archivelog all;
RMAN> delete expired backupset;

RMAN> catalog start with '/home/oracle/rac-backup/'; 

searching for all files that match the pattern /home/oracle/rac-backup/

List of Files Unknown to the Database
=====================================
File Name: /home/oracle/rac-backup/nnndf0_full_all_0.261.1092197861
File Name: /home/oracle/rac-backup/nnndf0_full_all_0.262.1092197797
File Name: /home/oracle/rac-backup/nnndf0_full_all_0.265.1092197639
File Name: /home/oracle/rac-backup/s_1092197886.293.1092197887

Do you really want to catalog the above files (enter YES or NO)? yes
cataloging files...
cataloging done

List of Cataloged Files
=======================
File Name: /home/oracle/rac-backup/nnndf0_full_all_0.261.1092197861
File Name: /home/oracle/rac-backup/nnndf0_full_all_0.262.1092197797
File Name: /home/oracle/rac-backup/nnndf0_full_all_0.265.1092197639
File Name: /home/oracle/rac-backup/s_1092197886.293.1092197887
```



## 设置数据文件

查看schema

```
RMAN> report schema;

RMAN-06139: WARNING: control file is not current for REPORT SCHEMA
Report of database schema for database with db_unique_name DEMODB

List of Permanent Datafiles
===========================
File Size(MB) Tablespace           RB segs Datafile Name
---- -------- -------------------- ------- ------------------------
1    0        SYSTEM               ***     +DATA/demodb/datafile/system.264.1081074747
2    0        SYSAUX               ***     +DATA/demodb/datafile/sysaux.259.1081074747
3    0        UNDOTBS1             ***     +DATA/demodb/datafile/undotbs1.258.1081074747
4    0        USERS                ***     +DATA/demodb/datafile/users.257.1081074747
5    0        UNDOTBS2             ***     +DATA/demodb/datafile/undotbs2.285.1081074909
6    0        XY                   ***     /u01/app/oracle/xy.dbf

List of Temporary Files
=======================
File Size(MB) Tablespace           Maxsize(MB) Tempfile Name
---- -------- -------------------- ----------- --------------------
1    20       TEMP                 32767       +DATA/demodb/tempfile/temp.284.1081074901

RMAN> 
```

利用File列和DatafileName列设置新名词

```
run {
set newname for datafile 1 to '/u01/app/oracle/oradata/restore/system.dbf';
set newname for datafile 2 to '/u01/app/oracle/oradata/restore/sysaux.dbf';
set newname for datafile 3 to '/u01/app/oracle/oradata/restore/undotbs1.dbf';
set newname for datafile 4 to '/u01/app/oracle/oradata/restore/users.dbf';
set newname for datafile 5 to '/u01/app/oracle/oradata/restore/undotbs2.dbf';
set newname for datafile 6 to '/u01/app/oracle/oradata/restore/xy.dbf';
SET NEWNAME FOR TEMPFILE 1 to '/u01/app/oracle/oradata/restore/temp01.dbf';
restore database;
SWITCH DATAFILE ALL;
SWITCH TEMPFILE ALL;
}
```

上方命令在rman下运行正常时，查看数据文件应有如下输出

```bash
[oracle@localhost rac-backup]$ ls -1 /u01/app/oracle/oradata/restore/
sysaux.dbf
system.dbf
undotbs1.dbf
undotbs2.dbf
users.dbf
xy.dbf
```

数据文件恢复后,RECOVER数据库

```
RMAN> recover database;
```

如果提示RMAN-06025: no backup of archived log for thread 1 with sequence 557 and starting SCN of 22227940 found to restore

在rac端备份归档日志或直接使用本文开头使用的归档日志，并复制到单节点的/home/rac-backup并在单节点注册单个备份片

```sql
-- 单节点
RMAN>CATALOG BACKUPPIECE  '/home/oracle/rac-backup/arch_1092202140_116_.dbf';
```

重新恢复数据库，只要ststem.dbf等数据文件没有报错，可以忽略 media recovery requesting unknown archived log for thread 1 with sequence 559 and starting SCN of 22232905类似报错

```sql
RMAN> recover database;
Database altered.
```

## 设置重做日志

设置重做日志文件路径，再输出结果交给sqlplus执行

```sql
-- 查看当前重做日志文件位置
SQL> set pagesize 100;
SQL> SET LINESIZE 150;
SQL> select member from v$logfile;
-- 设置新的重做日志路径
-- 24为重做日志文件所在的文件夹的字符长度
-- 如果尾部没有.log，可以用编辑器添加
SQL> select 'alter database rename file '''||member||''' to ''/u01/app/oracle/oradata/restore/'||substr(member,24)||'''' from v$logfile;


-- 执行重命名的语句
alter database rename file '+DATA/demodb/onlinelog/group_2.282.1081074893' to '/u01/app/oracle/oradata/restore/redo01.log';
alter database rename file '+DATA/demodb/onlinelog/group_2.283.1081074893' to '/u01/app/oracle/oradata/restore/redo02.log';
alter database rename file '+DATA/demodb/onlinelog/group_1.280.1081074893' to '/u01/app/oracle/oradata/restore/redo03.log';
alter database rename file '+DATA/demodb/onlinelog/group_1.281.1081074893' to '/u01/app/oracle/oradata/restore/redo04.log';
alter database rename file '+DATA/demodb/onlinelog/group_3.286.1081074975' to '/u01/app/oracle/oradata/restore/redo05.log';
alter database rename file '+DATA/demodb/onlinelog/group_3.287.1081074975' to '/u01/app/oracle/oradata/restore/redo06.log';
alter database rename file '+DATA/demodb/onlinelog/group_4.288.1081074975' to '/u01/app/oracle/oradata/restore/redo07.log';
alter database rename file '+DATA/demodb/onlinelog/group_4.289.1081074975' to '/u01/app/oracle/oradata/restore/redo08.log';

-- 执行后重新查看文件名
SQL> select member from v$logfile;
MEMBER
---------------------------------------------
/u01/app/oracle/oradata/restore/redo01.log
/u01/app/oracle/oradata/restore/redo02.log
/u01/app/oracle/oradata/restore/redo03.log
/u01/app/oracle/oradata/restore/redo04.log
/u01/app/oracle/oradata/restore/redo05.log
/u01/app/oracle/oradata/restore/redo06.log
/u01/app/oracle/oradata/restore/redo07.log
/u01/app/oracle/oradata/restore/redo08.log
```

如果有CLEARING_CURRENT的日志组，需要清除

```sql
SQL> select group#,thread#,status from v$log;
    GROUP#    THREAD# STATUS
---------- ---------- ----------------
	 1	    1 CLEARING
	 2	    1 CLEARING_CURRENT
	 3	    2 CLEARING
	 4	    2 CLEARING_CURRENT

SQL> alter database clear logfile group 2;
Database altered.

SQL> alter database clear logfile group 4;
Database altered.

-- 状态为CURRENT,表示日志正在使用
SQL> select group#,thread#,status from v$log;
    GROUP#    THREAD# STATUS
---------- ---------- ----------------
	 1	    1 CLEARING
	 2	    1 CURRENT
	 3	    2 CLEARING
	 4	    2 CURRENT
```

备份控制文件

```sql
SQL> alter database backup controlfile to trace as '/home/oracle/ctl.control';
```



## 启动单节点数据库

```sql
SQL> alter database open resetlogs;

Database altered.
```

