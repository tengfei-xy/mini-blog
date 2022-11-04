文档作者微信：SXL--LP（可联系获取安装包资源）

文档更新时间：2021年11月3日

# 一、补充：目标库DDL

如果目标库是空库，初始化之前需要先执行源库的DDL语句，

> 前置语句
>
> ```sql
> set long 99999         
> set pagesize 1000
> ```

1. 确定schema的表空间

   ```sql
   select DEFAULT_TABLESPACE,TEMPORARY_TABLESPACE from dba_users where username='TENGFEI';
   ```

2. 导出用户表空间DDL

   ```sql
   SELECT DBMS_METADATA.GET_DDL('TABLESPACE', TS.tablespace_name) 
   FROM DBA_TABLESPACES TS where ts.tablespace_name='XY';
   ```

3. 导出schemaDDL

   ```sql
   SELECT DBMS_METADATA.GET_DDL('USER',username) FROM DBA_USERS WHERE USERNAME='TENGFEI';
   
   -- GRANT UNLIMITED TABLESPACE TO <SCHEMA>
   -- GRANT CREATE SESSION TO <SCHEMA>
   ```

4. 连接业务用户后，导出表、索引、存储过程DDL

   ```sql
   SELECT DBMS_METADATA.GET_DDL(U.OBJECT_TYPE, u.object_name)
   FROM USER_OBJECTS u
   where U.OBJECT_TYPE IN ('TABLE','INDEX','PROCEDURE');
   ```

5. 备库禁用所有触发器

# 二、多端：基础配置

## 1、用户配置

1. 创建系统用户

   ```bash
   useradd -g oinstall -G dba -p `openssl passwd` ogg
   ```

2. 修改ogg用户的.bash_profile配置文件，别忘记生效文件

   ```ini
   PATH=/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin
   PATH=$PATH:$HOME/.local/bin:$HOME/bin
   
   export ORACLE_SID=oracle
   export ORACLE_BASE=/u01/app/oracle
   export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
   export LD_LIBRARY_PATH=$ORACLE_HOME/lib
   export PATH=$PATH:$ORACLE_HOME/bin:$ORACLE_HOME/jdk/bin
   ```

## 2、安装ogg

1. 创建OGG安装目录,并解压安装ogg文件

   ```bash
   mkdir /u01/app/ogg
   tar xf fbo_ggs_Linux_x64_ora11g_64bit.tar  -C /u01/app/ogg
   chown -R  ogg:oinstall /u01/app/ogg
   ```

2. 登录测试

   如果 LD_LIBRARY_PATH 变量设置正确，会像上面演示一样正确进入命令行，如果提示找不到 so 文件，就

   需要 查看 LD_LIBRARY_PATH 变量是否正确设置。

   ```bash
   [ogg@ogg2 ogg]$ ./ggsci 
   Oracle GoldenGate Command Interpreter for Oracle
   Version 11.2.1.0.10 17241368 OGGCORE_11.2.1.0.0OGGBP_PLATFORMS_130813.0048_FBO
   Linux, x64, 64bit (optimized), Oracle 11g on Aug 13 2013 06:04:43
   Copyright (C) 1995, 2013, Oracle and/or its affiliates. All rights reserved.
   ```
   
3. 创建ogg表空间,并创建用户

   ```sql
   SQL> create tablespace ogg datafile '/u01/app/oracle/oradata/ogg01.dbf' size 50M autoextend on;
   Tablespace created.
   SQL> create user ogg identified by ogg default tablespace ogg;
   User created.
   ```

4. 赋权

   连接资源、查找任何字典和表、在dmbs闪回表执行、闪回修改插入更新任何表、

   ```sql
   grant CONNECT, RESOURCE to ogg;
   grant SELECT ANY DICTIONARY to ogg;
   grant EXECUTE on DBMS_FLASHBACK to ogg;
   grant 
   select any table,
   flashback any table,
   insert any table,
   update any table,
   alter any table,
   delete any table to ogg;
   
   ```
   
5. 数据库操作

   启动到mount模式
   
   ```bash
   SQL> select open_mode,force_logging from v$database;
   OPEN_MODE	           FOR
   -------------------- ---
   READ WRITE	         NO
   
   
   SQL> shutdown immediate; 
   SQL> startup mount; 
   ```
   
   归档日志
   
   ```sql
   SQL> alter database force logging;
   SQL> alter database archivelog;
   Database altered.
   SQL> alter database open;
   ```
   
   附加日志
   
   ```sql
   SQL> select SUPPLEMENTAL_LOG_DATA_MIN,SUPPLEMENTAL_LOG_DATA_PK,SUPPLEMENTAL_LOG_DATA_UI from v$database;
   
   SUPPLEME SUP SUP
   -------- --- ---
   NO       NO  NO
   SQL> alter database add supplemental log data ;
   SQL> alter database add supplemental log data (primary key, unique,foreign key) columns;
   SQL> alter system switch logfile;
   -- 关闭附加日志:alter database drop supplemental log data (primary key,unique,foreign key) columns;
   ```
   
   
   
6. 使用sqlplus安装marker表和Security Role

   ```sql
   -- 进入ogg安装目录
   SQL> @marker_setup.sql
   SQL> @role_setup.sql
   SQL> GRANT GGS_GGSUSER_ROLE TO <loggedUser>;
   ```

7. 创建管理目录

   ```sql
   GGSCI (ogg1) 1>  create subdirs
   Creating subdirectories under current directory /u01/app/ogg
   
   Parameter files                /u01/app/ogg/dirprm: already exists
   Report files                   /u01/app/ogg/dirrpt: created
   Checkpoint files               /u01/app/ogg/dirchk: created
   Process status files           /u01/app/ogg/dirpcs: created
   SQL script files               /u01/app/ogg/dirsql: created
   Database definitions files     /u01/app/ogg/dirdef: created
   Extract data files             /u01/app/ogg/dirdat: created
   Temporary files                /u01/app/ogg/dirtmp: created
   Stdout files                   /u01/app/ogg/dirout: created
   ```

8. ==源端==：从sqplus创建的用户登录ggsci

    ```sql
    GGSCI (ogg1) 1> dblogin userid ogg,password ogg
    Successfully logged into database.
    
    # 源端添加表级TRANDATA
    GGSCI (ogg1) 2> add trandata tengfei.*
    Logging of supplemental redo data enabled for table TENGFEI.IDT.
    
    # 查看哪些表添加了TRANDATA
    GGSCI (ogg1) 3> info trandata tengfei.*
    Logging of supplemental redo log data is enabled for table TENGFEI.IDT.
    Columns supplementally logged for table TENGFEI.IDT: ID3, ID2, ID.
    ```

# 三、多端：配置与启动管理进程

==注：配置时需要在原先的文件夹下==

1. 配置MGR管理进程

    ```sql
    GGSCI (ogg1) 4> edit params mgr
    port 7809
    purgeoldextracts /u01/app/ogg/dirdat, usecheckpoints, minkeepdays 7
    LAGREPORTHOURS 1
    LAGINFOMINUTES 30
    LAGCRITICALMINUTES 45
    
    
    # PORT 7809：OGG 管理进程监控端口。
    # PURGEOLDEXTRACTS：清除不需要的 trail 文件。
    # /ogg/dirdat：trail 文件存放位置。
    # USECHECKPOINTS：使用检查点队列
    # minkeepdays 超过7天会删除
    # LAGREPORTHOURS 每隔1小时检查EXTRACT的延迟情况
    # LAGINFOMINUTES 如果超过了30分钟就把延迟作为信息记录到错误日志中
    # LAGCRITICALMINUTES如果延迟超过了45分钟，则把它作为警告写到错误日志中。
    # 自动重启 EXTRACT进程，表示每3分钟尝试重新启动所有EXTRACT进程，共尝试5次；
    # AUTORESTART EXTRACT *,RETRIES5,WAITMINUTES 3
    
    
    ```

2. 启动mgr

    ```sql
    GGSCI (ogg1) 5> start mgr
    Manager started.
    ```
    
3. 查看状态(如果不是下方的特定的消息可通过view report mgr查看错误)

    ```sql
    GGSCI (ogg1) 6> info mgr
    Manager is running (IP port ogg1.7809).
    ```



# 四、多端：配置与初始化进程

1. 源端添加EINI_1（extract进程）

   ```sql
   GGSCI (ogg1) 1> add extract eini_1, sourceistable
   EXTRACT added.
   # SOURCEISTABLE          Extracts entire records from source tables.
   ```

2. 目标端添加RINI_1（REPLICAT进程）

   ```sql
   GGSCI (ogg2) 1> ADD REPLICAT RINI_1, SPECIALRUN
   REPLICAT added.
   # 将生成./dirchk/RINI_1.cpr文件
   # SPECIALRUN             Specifies a one-time processing task that does not checkpoint from run to run.
   ```
   
3. 查看EINI_1的进程状态

   还没有配置和启动时，该进程是STOPPED状态

   ```sql
   GGSCI (ogg1) 9> INFO EXTRACT EINI_1
   
   EXTRACT    EINI_1    Initialized   2021-07-12 09:05   Status STOPPED
   Checkpoint Lag       Not Available
   Log Read Checkpoint  Not Available
                        First Record         Record 0
   Task                 SOURCEISTABLE
   ```

4. 源端：配置名为**EINI_1**的extract进程

   ==注：TABLE可以指定*号作为通配符==

   ```
   GGSCI (ogg1) 10> edit params eini_1
   
   extract EINI_1
   setenv (NLS_LANG=AMERICAN_AMERICA.ZHS16GBK)
   userid ogg, PASSWORD ogg
   rmthost 192.168.238.38, mgrport 7809
   rmttask replicat, group RINI_1
   TABLE tengfei.idt;

5. 目标端：配置名为**RINI_1**的REPLICAT进程

   ==注：map与target可以指定*号作为通配符==
   
   ```bash
   GGSCI (ora2) 6> edit params rini_1
   
   REPLICAT RINI_1
   setenv (NLS_LANG=AMERICAN_AMERICA.ZHS16GBK)
   assumetargetdefs
   userid ogg,password ogg
   discardfile /u01/app/ogg/RINIaa.dsc, purge
   map tengfei.idt, target tengfei.idt;
   
   # assumetargetdefs 表示源与目标的表具有相同结构
   # discardfile purge 写文件之前会清除内容
   ```
   

==注：初始化数据工作只会做一次，目标端Rini_1不用手动启动==

6. 源端启动

   ```
   GGSCI (ogg1) 2> start extract eini_1
   Sending START request to MANAGER ...
   EXTRACT EINI_1 starting
   ```

7. 查看进程

   ```
   GGSCI (ogg1) 3> view report eini_1
   ```

执行完后ggserr.log将显示：

> 2021-07-16 11:25:58  INFO    OGG-00991  Oracle GoldenGate Capture for Oracle, eini_1.prm:  EXTRACT EINI_1 stopped normally.

# 五、多端：配置检查点

为了让 OGG 网络中断、服务器宕机、掉电等在突发情况也能正确断点续传，ORACLE 建议配置 OGG 的检查点队列。[参考文档](http://www.dba-oracle.com/t_goldengate_checkpoint_table.htm)

1. 源端和目标端配置并添加信息

   ```
   GGSCI (stream) 2> EDIT PARAMS ./GLOBALS
   CHECKPOINTTABLE ogg.ggschkpt
   ```

2. 使用ogg用户登录数据库

   ```
   dblogin userid ogg,password ogg 
   ```

3. 创建检查点

   ```
   add checkpointtable ogg.ggschkpt
   ```

4. 从SQLPLUS使用ogg用户登录并查看检查点表

   ```sql
   SQL> conn ogg/ogg
   SQL> select * from tab;
   
   TNAME			                     TABTYPE	CLUSTERID
   ------------------------------ ------- ----------
   GGSCHKPT		                   TABLE
   GGSCHKPT_LOX		               TABLE
   GGS_DDL_COLUMNS 	             TABLE
   GGS_DDL_HIST		               TABLE
   GGS_DDL_HIST_ALT	             TABLE
   GGS_DDL_LOG_GROUPS	           TABLE
   GGS_DDL_OBJECTS 	             TABLE
   GGS_DDL_PARTITIONS	           TABLE
   GGS_DDL_PRIMARY_KEYS	         TABLE
   GGS_DDL_RULES		               TABLE
   GGS_DDL_RULES_LOG	             TABLE
   
   TNAME			                     TABTYPE	CLUSTERID
   ------------------------------ ------- ----------
   GGS_MARKER		                 TABLE
   GGS_SETUP		                   TABLE
   GGS_STICK		                   TABLE
   GGS_TEMP_COLS		               TABLE
   GGS_TEMP_UK		                 TABLE
   
   16 rows selected.
   ```

# 六、源端：配置Extract进程

## 1、捕获进程

1. 源端配置EORA_1进程：

   ```sql
   GGSCI (ogg1) 1> edit params eora_1
   EXTRACT EORA_1
   SETENV (NLS_LANG=AMERICAN_AMERICA.ZHS16GBK)
   USERID ogg, PASSWORD ogg
   EXTTRAIL /u01/app/ogg/dirdat/aa
   TABLE tengfei.idt;
   ```
   
   EXTTRAIL 为端的dirdat路径
   
2. 开始捕获

   ```sql
   GGSCI (ogg1) 2> add extract eora_1, tranlog, begin now
   EXTRACT added.
   ## 队列大小5M
   GGSCI (ogg1) 3> add exttrail /u01/app/ogg/dirdat/aa, extract eora_1, megabytes 5
   EXTTRAIL added.
   ```

   EXTTRAIL 参数是 TRAIL 队列文件存放的路径和命名格式

   TRAIL 文件可以理解为存放捕获进程捕获的日志文件

3. 启动进程

   ```sql
   GGSCI (ogg1) 4> start extract eora_1
   
   Sending START request to MANAGER ...
   EXTRACT EORA_1 starting
   ```

   查看进程：`info extract eora_1`

## 2、PUMP传输进程

1. 源端配置

   此步骤也是非必须的，如果不配置传输进程，OGG 会通过 EXTRACT 进程传输 TRAIL 队列文件，但是和检查点队列一样，为了保证断点续传 ORACLE 建议配置 PUMP 传输进程。

   ```
   GGSCI (ogg1) 2> EDIT PARAMS PORA_1
   EXTRACT PORA_1
   SETENV (NLS_LANG=AMERICAN_AMERICA.ZHS16GBK)
   PASSTHRU
   RMTHOST 192.168.249.38, MGRPORT 7809
   RMTTRAIL /u01/app/ogg/dirdat/pa
   TABLE tengfei.idt;
   ```

2. 添加pump进程到OGG，并指定本地的TRAIL文件

   ```
    GGSCI (ogg1) 3> ADD EXTRACT PORA_1, EXTTRAILSOURCE /u01/app/ogg/dirdat/aa
   ```

3. 查看状态

   ```
   GGSCI (ogg1) 4> INFO EXTRACT PORA_1
   
   EXTRACT    PORA_1    Initialized   2021-08-05 14:38   Status STOPPED
   Checkpoint Lag       00:00:00 (updated 00:00:35 ago)
   Log Read Checkpoint  File //u01/app/ogg/dirdat/aa000000
                        First Record  RBA 0
   ```

4. 为 PUMP 进程 PORA_1 指定将本地 TRAIL 文件传输到目标端后保存成目标端 TRAIL 文件的名字

   ```
   GGSCI (ogg1) 5> ADD RMTTRAIL /u01/app/ogg/dirdat/pa, EXTRACT PORA_1, MEGABYTES 100
   RMTTRAIL added.
   ```

5. 启动进程

   ```sql
   GGSCI (ogg1) 6> start extract pora_1
   Sending START request to MANAGER ...
   EXTRACT PORA_1 starting
   ```

6. 查看状态

   ```
   GGSCI (ogg1) 5> info extract pora_1
   
   EXTRACT    PORA_1    Last Started 2021-08-05 14:54   Status RUNNING
   Checkpoint Lag       00:00:00 (updated 00:00:07 ago)
   Log Read Checkpoint  File /u01/app/ogg/dirdat/aa000000
                        First Record  RBA 1043
   ```

   注：当状态为abended时，可通过查看ggserr.log检查报错

   注：正常情况将在目标端dirdata文件夹下生成pa000000文件
   

# 七、目标端：配置Replicat进程

1. 添加进程

   ```
   GGSCI (ogg2) 1>  ADD REPLICAT RRA_1, exttrail /u01/app/ogg/dirdat/pa, checkpointtable ogg.ggschkpt
   REPLICAT added.
   ```

2. 编辑进程

   ```
   GGSCI (ogg2) 2> edit params rora_1
   REPLICAT RORA_1
   SETENV (NLS_LANG=AMERICAN_AMERICA.ZHS16GBK)
   USERID ogg, PASSWORD ogg
   ASSUMETARGETDEFS
   DISCARDFILE /u01/app/ogg/dirrpt/RORA_aa.DSC, PURGE
   MAP tengfei.idt, TARGET tengfei.idt;
   ```
   
3. 启动进程

   ```
   GGSCI (ogg2) 3> start replicat rora_1
   Sending START request to MANAGER ...
   REPLICAT RORA_1 starting
   ```

4. 查看进程（running状态正常）

   ```
   GGSCI (ogg2) 4> info replicat rora_1
   ```
   
   



到此配置完成，可检查数据同步情况！
