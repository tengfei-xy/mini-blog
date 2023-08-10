# Windows-12.1.0经典模式

## 目录

-   [安装ogg](#安装ogg)
-   [补充：目标库DDL](#补充目标库DDL)
-   [一、双端：基础配置](#一双端基础配置)
-   [二、双端：配置与启动管理进程与服务](#二双端配置与启动管理进程与服务)
    -   [配置服务](#配置服务)
    -   [配置进程参数](#配置进程参数)
-   [三、双端：配置与初始化进程](#三双端配置与初始化进程)
-   [四、配置检查点](#四配置检查点)
-   [五、源端：配置捕获、PUMP进程](#五源端配置捕获PUMP进程)
    -   [1. 配置捕获进程](#1-配置捕获进程)
    -   [2. PUMP传输进程](#2-PUMP传输进程)
-   [六、目标端配置同步进程](#六目标端配置同步进程)

# 安装ogg

1.  运行setup.exe进行安装ogg
2.  安装完成后，运行ggsci.exe
3.  在sysdm.cpl设定ORACLE\_SID和ORALCE\_HOME

# 补充：目标库DDL

如果目标库是空库，初始化之前需要先执行源库的DDL语句，

> 前置语句

```sql
set long 99999         
set pagesize 1000
```

1.  确定schema的表空间
    ```sql
    select DEFAULT_TABLESPACE,TEMPORARY_TABLESPACE from dba_users where username='TENGFEI';
    ```
2.  导出用户表空间DDL
    ```sql
    SELECT DBMS_METADATA.GET_DDL('TABLESPACE', TS.tablespace_name) 
    FROM DBA_TABLESPACES TS where ts.tablespace_name='XY';
    ```
3.  导出schemaDDL
    ```sql
    SELECT DBMS_METADATA.GET_DDL('USER',username) FROM DBA_USERS WHERE USERNAME='TENGFEI';

    -- GRANT UNLIMITED TABLESPACE TO <SCHEMA>
    -- GRANT CREATE SESSION TO <SCHEMA>
    ```
4.  连接业务用户后，导出表、索引、存储过程DDL
    ```sql
    SELECT DBMS_METADATA.GET_DDL(U.OBJECT_TYPE, u.object_name)
    FROM USER_OBJECTS u
    where U.OBJECT_TYPE IN ('TABLE','INDEX','PROCEDURE');
    ```

# 一、双端：基础配置

1.  打开数据库附加日志和force log
    ```sql
    SQL> select open_mode,force_logging,supplemental_log_data_min from v$database;
    OPEN_MODE	           FOR SUPPLEME
    -------------------- --- --------
    READ WRITE	         NO NO


    SQL> shutdown immediate; 
    SQL> startup mount; 
    SQL> alter database force logging;
    SQL> alter database archivelog;
    SQL> alter database open;
    SQL> alter database add supplemental log data;
    -- 关闭补充日志:alter database drop supplemental log data;
    ```
2.  创建ogg表空间,并创建用户
    ```sql
    SQL> create tablespace ogg datafile 'C:\app\Administrator\oradata\ogg01.dbf' size 50M autoextend on;
    Tablespace created.
    SQL> create user ogg identified by ogg default tablespace ogg;
    User created.
    ```
3.  使用sqlplus安装marker表和Security Role
    ```sql
    -- 进入ogg安装目录
    SQL> @marker_setup.sql
    SQL> @role_setup.sql
    SQL> GRANT GGS_GGSUSER_ROLE TO <loggedUser>;
    ```
4.  赋权
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
5.  创建管理目录
    ```sql
    GGSCI (WIN-QHLFMQGHCTV) 3> create subdirs

    Creating subdirectories under current directory C:\ogg

    Parameter file                 C:\app\Administrator\product\12.1.2\oggcore_1\dirprm: created.
    Report file                    C:\app\Administrator\product\12.1.2\oggcore_1\dirrpt: created.
    Checkpoint file                C:\app\Administrator\product\12.1.2\oggcore_1\dirchk: created.
    Process status files           C:\app\Administrator\product\12.1.2\oggcore_1\dirpcs: created.
    SQL script files               C:\app\Administrator\product\12.1.2\oggcore_1\dirsql: created.
    Database definitions files     C:\app\Administrator\product\12.1.2\oggcore_1\dirdef: created.
    Extract data files             C:\app\Administrator\product\12.1.2\oggcore_1\dirdat: created.
    Temporary files                C:\app\Administrator\product\12.1.2\oggcore_1\dirtmp: created.
    Credential store files         C:\app\Administrator\product\12.1.2\oggcore_1\dircrd: created.
    Masterkey wallet files         C:\app\Administrator\product\12.1.2\oggcore_1\dirwlt: created.
    Dump files                     C:\app\Administrator\product\12.1.2\oggcore_1\dirdmp: created.
    ```
6.  \==源端==：从sqplus创建的用户登录ggsci
    ```sql
    GGSCI (WIN-QHLFMQGHCTV) 1> dblogin userid ogg,password ogg
    Successfully logged into database.

    # 源端添加表级TRANDATA
    GGSCI (WIN-QHLFMQGHCTV) 2> add trandata tengfei.*
    Logging of supplemental redo data enabled for table TENGFEI.IDT.

    # 查看哪些表添加了TRANDATA
    GGSCI (WIN-QHLFMQGHCTV) 3> info trandata tengfei.*
    Logging of supplemental redo log data is enabled for table TENGFEI.IDT.
    Columns supplementally logged for table TENGFEI.IDT: ID3, ID2, ID.
    ```

# 二、双端：配置与启动管理进程与服务

## 配置服务

1.  进入安装目录，配置全局文件，指定mgr的服务名
    ```纯文本
    EDIT PARAMS GLOBALS
    ```
2.  使用ogg-mgr作为服务名(如不指定默认使用GGSMGR)
    ```纯文本
    MGRSERVNAME ogg-mgr
    ```
3.  进入安装目录，使用管理员打开命令行
    ```纯文本
    .\install.exe ADDSERVICE
    ```
4.  设定为自动启动
    ```纯文本
    .\install.exe AUTOSTART
    ```
5.  判断安装情况
    ```powershell
    PS C:\ogg> GET-Service -Name ogg-mgr

    Status   Name               DisplayName
    ------   ----               -----------
    Stopped  ogg-mgr             ogg-mgr
    ```

其他运行方式参考：

| 选项                          | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ADDEVENTS                   | 将Oracle GoldenGate事件添加到Windows事件管理器中。                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ADDSERVICE                  | 将管理器添加为服务，其名称由`GLOBALS`文件中的`MGRSERVNAME`参数（如果有的话）或`GGSMGR`默认值指定。ADDSERVICE将服务配置为本地系统帐户运行，这是大多数Windows应用程序的标准，因为该服务可以独立于用户登录和密码更改运行。要将管理器作为特定帐户运行，请使用`USER`和`PASSWORD`选项。[](https://docs.oracle.com/en/middleware/goldengate/core/19.1/installing/installing-manager-windows-service.html#fn_1)[Foot 1](https://docs.oracle.com/en/middleware/goldengate/core/19.1/installing/installing-manager-windows-service.html#fn_1 "Foot 1")安装该服务是为了在系统启动时启动（请参阅`AUTOSTART`）。要在安装后启动它，请重新启动系统或从控制面板中的服务小程序手动启动服务。 |
| AUTOSTART                   | 设置使用`ADDSERVICE`创建的服务，以便在系统启动时启动。除非使用`MANUALSTART`，否则这是默认值。                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| MANUALSTART                 | 设置使用`ADDSERVICE`创建的服务，以通过控制面板中的GGSCI、脚本或服务小程序手动启动。默认是`AUTOSTART`。                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `USER` \*\**`name`*         | 指定执行管理器的域用户帐户。对于\*`name`\*，包括域名、向后斜杠和用户名，例如`HEADQT\GGSMGR`。默认情况下，安装管理器服务以使用本地系统帐户。                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `PASSWORD` \*\**`password`* | 为使用`USER`指定的用户指定密码。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## 配置进程参数

\==注：配置时需要在原先的文件夹下==

1.  配置MGR管理进程
    ```sql
    GGSCI (ogg1) 4> edit params mgr
    # 添加以下内容
    # PORT 7809：OGG 管理进程监控端口。
    # PURGEOLDEXTRACTS：清除不需要的 trail 文件。
    # /ogg/dirdat：trail 文件存放位置。
    # USECHECKPOINTS：使用检查点队列。
    port 7809
    purgeoldextracts C:\app\Administrator\product\12.1.2\oggcore_1\dirdat, usecheckpoints
    ```
2.  启动mgr
    ```sql
    GGSCI (ogg1) 5> start mgr
    Manager started.
    ```
3.  查看状态(如果不是下方的特定的消息可通过view report mgr查看错误)
    ggsci方式：
    ```sql
    GGSCI (ogg1) 6> info mgr
    Manager is running (IP port ogg1.7809).
    ```
    ps方式：
    ```powershell
    PS C:\Users\Administrator> get-service -name ogg-mgr

    Status   Name               DisplayName
    ------   ----               -----------
    Running  ogg-mgr            ogg-mgr
    ```

# 三、双端：配置与初始化进程

1.  源端添加EINI\_1（extract进程）
    ```sql
    GGSCI (WIN-QHLFMQGHCTV) 1> add extract eini_1, sourceistable
    # 参考命令： add extract eini_1 tranlog,begin now.
    EXTRACT added.
    ```
2.  目标端添加RINI\_1（REPLICAT进程）
    ```sql
    GGSCI (ogg2) 1> ADD REPLICAT RINI_1, SPECIALRUN
    REPLICAT added.
    # 将生成./dirchk/RINI_1.cpr文件
    ```
3.  查看EINI\_1的进程状态
    还没有配置和启动时，该进程是STOPPED状态
    ```sql
    GGSCI (ogg1) 9> INFO EXTRACT EINI_1

    EXTRACT    EINI_1    Initialized   2021-07-12 09:05   Status STOPPED
    Checkpoint Lag       Not Available
    Log Read Checkpoint  Not Available
                         First Record         Record 0
    Task                 SOURCEISTABLE
    ```
4.  源端：配置名为**EINI\_1**的extract进程
    \==注：TABLE可以指定\*号作为通配符,或具体表==
    ```纯文本
    GGSCI (ogg1) 10> edit params eini_1

    extract EINI_1
    setenv (NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK)
    userid ogg, PASSWORD ogg
    rmthost 192.168.245.44, mgrport 7809
    rmttask replicat, group RINI_1
    TABLE tengfei.*;
    ```
5.  目标端：配置名为**RINI\_1**的REPLICAT进程
    \==注：map与target可以指定\*号作为通配符,或具体表==
    ```纯文本
    GGSCI (ora2) 6> edit params rini_1

    REPLICAT RINI_1
    setenv (NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK)
    assumetargetdefs
    userid ogg,password ogg
    discardfile C:\app\Administrator\product\12.1.2\oggcore_1\RINIaa.dsc, purge
    map tengfei.*, target tengfei.*;
    ```

\==注：初始化数据工作只会做一次，目标端Rini\_1不用手动启动==

1.  源端启动
    ```纯文本
    GGSCI (ogg1) 2> start extract eini_1
    Sending START request to MANAGER ...
    EXTRACT EINI_1 starting
    ```
2.  查看进程
    ```纯文本
    GGSCI (WIN-QHLFMQGHCTV) 3> view report eini_1
    ```
3.  执行完后ggerr.log：
    ```powershell
    # 输出尾部40行
    PS C:\ogg> Get-Content .\ggserr.log -Tail 40
    # 持续加载
    PS C:\ogg> Get-Content .\ggserr.log -wait
    ```

> INFO    OGG-00991  Oracle GoldenGate Capture for Oracle, eini\_1.prm:  EXTRACT EINI\_1 stopped normally.

# 四、配置检查点

为了让 OGG 网络中断、服务器宕机、掉电等在突发情况也能正确断点续传，ORACLE 建议配置 OGG 的检查点队列。[参考文档](http://www.dba-oracle.com/t_goldengate_checkpoint_table.htm "参考文档")

1.  源端和目标端配置并添加信息
    ```纯文本
    GGSCI (stream) 2> EDIT PARAMS ./GLOBALS
    CHECKPOINTTABLE ogg.ggschkpt
    ```
2.  使用ogg用户登录数据库
    ```纯文本
    dblogin userid ogg,password ogg 
    ```
3.  创建检查点
    ```纯文本
    add checkpointtable ogg.ggschkpt
    ```
4.  从SQLPLUS使用ogg用户登录并查看检查点表
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

# 五、源端：配置捕获、PUMP进程

## 1. 配置捕获进程

1.  源端配置EORA\_1进程：
    ```sql
    GGSCI (ogg1) 1> edit params eora_1

    EXTRACT EORA_1
    -- 需要确定语言select userenv('language') from dual;
    SETENV (NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK)
    USERID ogg, PASSWORD ogg
    EXTTRAIL C:\app\Administrator\product\12.1.2\oggcore_1\aa
    -- 多个表用*号
    TABLE tengfei.idt;
    ```
    EXTTRAIL 为端的dirdat路径
2.  开始捕获
    TRAIL 文件可以理解为存放捕获进程捕获的日志文件
    ```sql
    GGSCI (ogg1) 2> add extract eora_1, tranlog, begin now
    EXTRACT added.
    GGSCI (ogg1) 3> add exttrail C:\app\Administrator\product\12.1.2\oggcore_1\aa, extract eora_1, megabytes 5
    EXTTRAIL added.
    ```
    EXTTRAIL 参数是 TRAIL 队列文件存放的路径和命名格式
3.  启动进程
    ```sql
    GGSCI (ogg1) 4> start extract eora_1

    Sending START request to MANAGER ...
    EXTRACT EORA_1 starting
    ```
    查看进程：`info extract eora_1`

## 2. PUMP传输进程

1.  源端配置
    此步骤也是非必须的，如果不配置传输进程，OGG 会通过 EXTRACT 进程传输 TRAIL 队列文件，但是和检查点队列一样，为了保证断点续传 ORACLE 建议配置 PUMP 传输进程。
    ```纯文本
    GGSCI (ogg1) 2> EDIT PARAMS PORA_1
    EXTRACT PORA_1
    SETENV (NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK)
    PASSTHRU
    RMTHOST 192.168.245.44, MGRPORT 7809
    RMTTRAIL C:\app\Administrator\product\12.1.2\oggcore_1\pa
    TABLE tengfei.idt;
    ```
2.  添加pump进程到OGG，并制定本地的TRAIL文件
    ```纯文本
     GGSCI (ogg1) 3> ADD EXTRACT PORA_1, EXTTRAILSOURCE C:\app\Administrator\product\12.1.2\oggcore_1\aa
    ```
3.  查看状态
    ```纯文本
    GGSCI (ogg1) 4> INFO EXTRACT PORA_1

    EXTRACT    PORA_1    Initialized   2021-08-05 14:38   Status STOPPED
    Checkpoint Lag       00:00:00 (updated 00:00:35 ago)
    Log Read Checkpoint  File //u01/app/ogg/dirdat/aa000000
                         First Record  RBA 0
    ```
4.  为 PUMP 进程 PORA\_1 指定将本地 TRAIL 文件传输到目标端后保存成目标端 TRAIL 文件的名字
    ```纯文本
    GGSCI (ogg1) 5> ADD RMTTRAIL C:\app\Administrator\product\12.1.2\oggcore_1\pa, EXTRACT PORA_1, MEGABYTES 5
    RMTTRAIL added.
    ```
5.  启动进程
    ```sql
    GGSCI (ogg1) 6> start extract pora_1
    Sending START request to MANAGER ...
    EXTRACT PORA_1 starting
    ```
6.  查看状态
    注：正常情况将在目标端dirdata文件夹下生成pa000000文件
    ```纯文本
    GGSCI (ogg1) 5> info extract pora_1

    EXTRACT    PORA_1    Last Started 2021-08-05 14:54   Status RUNNING
    Checkpoint Lag       00:00:00 (updated 00:00:07 ago)
    Log Read Checkpoint  File /u01/app/ogg/dirdat/aa000000
                         First Record  RBA 1043
    ```
    注：当状态为abended时，可通过查看ggserr.log检查报错

# 六、目标端配置同步进程

1.  添加进程
    ```纯文本
    GGSCI (ogg2) 1>  ADD REPLICAT RORA_1, exttrail C:\app\Administrator\product\12.1.2\oggcore_1\pa, checkpointtable ogg.ggschkpt
    ```
2.  编辑进程
    ```纯文本
    GGSCI (ogg2) 2> edit params rora_1
    REPLICAT RORA_1
    SETENV (NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK)
    USERID ogg, PASSWORD ogg
    HANDLECOLLISIONS
    ASSUMETARGETDEFS
    DISCARDFILE C:\app\Administrator\product\12.1.2\oggcore_1\RORA_aa.DSC, PURGE
    MAP tengfei.idt, TARGET tengfei.idt;
    ```
3.  启动进程
    ```纯文本
    GGSCI (ogg2) 3> start replicat rora_1
    Sending START request to MANAGER ...
    REPLICAT RORA_1 starting
    ```
4.  查看进程
    ```纯文本
    GGSCI (ogg2) 4> info replicat rora_1

    REPLICAT   RORA_1    Initialized   2021-08-05 14:58   Status STOPPED
    Checkpoint Lag       00:00:00 (updated 00:02:56 ago)
    Log Read Checkpoint  Not Available
    Task                 SPECIALRUN
    ```

到此配置完成，可检查数据同步情况！
