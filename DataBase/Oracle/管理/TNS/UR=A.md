[原文参考链接](http://feed.askmaclean.com/archives/tnsname%E4%B8%ADura%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8.html)

当数据库nomount,mount或者restricted时，动态监听显示状态为BLOCKED时，客户端可通过配置UR=A进行连接。

## 修改数据库状态

```
SQL> startup nomount;
ORACLE instance started.

Total System Global Area  939495424 bytes
Fixed Size                  2233960 bytes
Variable Size             562039192 bytes
Database Buffers          369098752 bytes
Redo Buffers                6123520 bytes
SQL> ALTER SYSTEM REGISTER;

System altered.

[oracle@rhl6 ~]$ lsnrctl status

LSNRCTL for Linux: Version 11.2.0.3.0 - Production on 15-JUL-2014 08:41:00

Copyright (c) 1991, 2011, Oracle.  All rights reserved.

Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.3.0 - Production
Start Date                15-JUL-2014 08:40:23
Uptime                    0 days 0 hr. 0 min. 37 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Log File         /u01/app/oracle/diag/tnslsnr/rhl6/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=rhl6.0x64)(PORT=1521)))
Services Summary...
Service "ora11gr2" has 1 instance(s).
  Instance "ora11gr2", status BLOCKED, has 1 handler(s) for this service...
The command completed successfully
```

客户端配置

```
bbed_ur =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.137.154)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = ora11gr2)
	  (UR=A)
    )
  )


bbed =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.137.154)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = ora11gr2)
    )
  )
```

## 测试登录

使用bbed登录

```
C:\Users\YallonKing>sqlplus sys/oracle@bbed as sysdba

SQL*Plus: Release 11.2.0.1.0 Production on 星期二 7月 29 14:15:24 2014

Copyright (c) 1982, 2010, Oracle.  All rights reserved.

ERROR:
ORA-12528: TNS: 监听程序: 所有适用例程都无法建立新连接

```

使用bbed_ur登录

```
请输入用户名:
C:\Users\YallonKing>sqlplus sys/oracle@bbed_ur as sysdba

SQL*Plus: Release 11.2.0.1.0 Production on 星期二 7月 29 14:15:28 2014

Copyright (c) 1982, 2010, Oracle.  All rights reserved.


连接到:
Oracle Database 11g Enterprise Edition Release 11.2.0.3.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options

SQL> select status from v$instance;

STATUS
------------------------
STARTED

SQL> alter database mount;

数据库已更改。

SQL> alter database open;

数据库已更改。

SQL> select open_mode from v$database;

OPEN_MODE
--------------------
READ WRITE

SQL> select status from v$instance;

STATUS
------------
OPEN
```