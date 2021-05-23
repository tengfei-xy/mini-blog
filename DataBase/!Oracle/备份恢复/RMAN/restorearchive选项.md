# Restore archivelog选项

1.RAC环境下需要加上Thread Number，不加默认是Thread 1

```
RMAN> restore archivelog from sequence 112 thread 2;
```

2.恢复全部归档日志文件

```
RMAN> restore archivelog all;
```

3.恢复Sequence范围

```
RMAN> restore archivelog from sequence 90 until sequence 98;
RMAN> restore archivelog sequence between 20 and 35;
```

4.恢复从第5个归档日志起

```
RMAN> restore archivelog from sequence 5;
RMAN> restore archivelog low sequence 5;
```

5.恢复7天内的归档日志

```
RMAN> restore archivelog from time 'sysdate-7';
```

6.恢复到哪个日志文件为止

```
RMAN> restore archivelog until sequence 110;
RMAN> restore archivelog high sequence 108;
```

7.改变恢复到另外路径下 则可用下面语句

```
set archivelog destination to '/u01/backup';

RMAN> run
{allocate channel c1 type disk;
set archivelog destination to '/u01/backup';
restore archivelog all;
release channel c1;
}
```



8.根据时间查看需要的备份集：

```
RMAN> list backup of archivelog time between "to_date('2014-04-24 11:00:00','yyyy-mm-dd hh24:mi:ss')" and "to_date('2014-04-24 14:30','yyyy-mm-dd hh24:mi:ss')";
```

恢复指定时间段

```
RMAN> run {
set archivelog destination to '/u01/backup';
SQL 'ALTER SESSION SET NLS_DATE_FORMAT="YYYY-MM-DD HH24:MI:SS"';
restore archivelog time between '2014-04-24 11:00:00' and '2014-04-24 15:00:00';
}
```

