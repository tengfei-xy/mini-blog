rac进行备份

```bash
#!/bin/bash   
. ~/.bash_profile
export ORACLE_SID=rac1
BIN=$ORACLE_HOME/bin
LEVEL=$1
DATE=$(date +%Y%m%d)
BACKUP_PATH=/home/oracle/rac-backup
mkdir -p $BACKUP_PATH/"$LEVEL"/"$DATE"/
$BIN/rman log $BACKUP_PATH/"$LEVEL"/"$DATE"/rman_"$DATE".log << EOF
connect target /;   
run{ 
allocate channel c1 device type disk;
allocate channel c2 device type disk;
crosscheck archivelog all;
DELETE NOPROMPT expired archivelog ALL;
backup archivelog  all format '$BACKUP_PATH/$LEVEL/$DATE/archlog.%d.level.$LEVEL.%U_%T' delete all input; 
release channel c1; 
release channel c2; 
}
run{   
allocate channel c1 device type disk; 
allocate channel c2 device type disk;
crosscheck backupset of database; 
backup incremental level $LEVEL database format '$BACKUP_PATH/$LEVEL/$DATE/data.%d.level.$LEVEL.%U_%T'; 
backup spfile tag='spfile' format '$BACKUP_PATH/$LEVEL/$DATE/spfile_%U_%T'; 
backup current controlfile tag='control' format='$BACKUP_PATH/$LEVEL/$DATE/control_%U_%T'; 
delete noprompt expired backupset of database; 
delete noprompt obsolete; 
release channel c1; 
release channel c2; 
}
exit;
EOF
```

复制文件到单节点(/home/oracle/rac-backup)

```
control_0q0edg65_1_1_20211117
data.RACDB.level.0.0l0edg5q_1_1_20211117
data.RACDB.level.0.0m0edg5q_1_1_20211117
rman_20211117.log
spfile_0p0edg64_1_1_20211117
```

启动备份的spfile

```sql
RMAN>  restore spfile to '/home/oracle/spfile' from '/home/oracle/rac-backup/spfile_0p0edg64_1_1_20211117';
```

如果目标库提示未备份在目标库(mount状态)上执行

```
backup database
CONFIGURE CONTROLFILE AUTOBACKUP ON;
CONFIGURE CONTROLFILE AUTOBACKUP FORMAT FOR DEVICE TYPE DISK TO '%F'; 
CONFIGURE CONTROLFILE AUTOBACKUP FORMAT FOR DEVICE TYPE 'SBT_TAPE' TO '%d_%I_%F';
```

将spfile转化为pfile

```sql
SQL> create pfile='/home/oracle/spfile' from spfile='/home/oracle/spfile';
```

处理pfile文件内容： audit_file_dest, background_dump_dest, core_dump_dest,user_dump_dest, log_archive_dest_1等，与路径有关的参数，修改为你单实例环境的对应路径。并移除cluster_database_instances，cluster_database参数 并创建下列文件夹

```
grep "/.*" /home/oracle/pfile -o | sed "s#'#\n#g" | grep "^/.*" -o
```

启动

```sql
SQL> startup nomount pfile='/home/oracle/pfile'
```

保存参数文件

```sql
SQL> create spfile from pfile='/home/oracle/pfile';
```

恢复控制文件

```sql
RMAN> set dbid=1340406187
RMAN> restore controlfile from '/home/oracle/rac-backup/control_0q0edg65_1_1_20211117';
```

启动到mount

```sql
RMAN>  alter database mount;
```

设置新数据文件名

```
SQL> set pagesize 100;
SQL> set line 200;
SQL> select 'set newname for datafile '''||name||''' to ''/u01/app/oracle/product/oradata/'||substr(name,21)||'''' from v$datafile; 
```

以上的直接结果加上（在数据库mount状态执行）

```
run {
ALLOCATE CHANNEL ch00 DEVICE TYPE disk;
ALLOCATE CHANNEL ch01 DEVICE TYPE disk;
ALLOCATE CHANNEL ch02 DEVICE TYPE disk;
ALLOCATE CHANNEL ch03 DEVICE TYPE disk;
set newname for datafile '/u01/app/oracle/oradata/racdb/system01.dbf' to '/u01/app/oracle/product/oradata/ata/racdb/system01.dbf';
set newname for datafile '/u01/app/oracle/oradata/racdb/sysaux01.dbf' to '/u01/app/oracle/product/oradata/ata/racdb/sysaux01.dbf';
set newname for datafile '/u01/app/oracle/oradata/racdb/undotbs01.dbf' to '/u01/app/oracle/product/oradata/ata/racdb/undotbs01.dbf';
set newname for datafile '/u01/app/oracle/oradata/racdb/users01.dbf' to '/u01/app/oracle/product/oradata/ata/racdb/users01.dbf';
set newname for datafile '/u01/app/oracle/oradata/racdb/undotbs02.dbf' to '/u01/app/oracle/product/oradata/ata/racdb/undotbs02.dbf';
restore database;
switch datafile all; 
RELEASE CHANNEL ch00;
RELEASE CHANNEL ch01;
RELEASE CHANNEL ch02;
RELEASE CHANNEL ch03;
}
```





[设置新归档日志](https://blog.csdn.net/a743044559/article/details/77847069)

```
SQL> select ' alter database rename file '''||member||''' to '' /u01/app/oracle/product/oradata'||substr(member,21)||'''' from v$logfile;
```

