# name

## 目录

-   [DB\_NAME](#DB_NAME)
-   [DBID](#DBID)
-   [DB\_UNIQUE\_NAME](#DB_UNIQUE_NAME)
-   [INSTANCE\_NAME](#INSTANCE_NAME)
-   [SID](#SID)
-   [SERVICE\_NAME](#SERVICE_NAME)
-   [GLOBAL\_DATABASE\_NAME](#GLOBAL_DATABASE_NAME)

## DB\_NAME

创建：在dbca时确定

1.  是数据库名，长度不能超过8个字符，记录在datafile、redolog和control file中
2.  在DataGuard环境中DB\_NAME相同而DB\_UNIQUE\_NAME不同
3.  在RAC环境中，各个节点的DB\_NAME 都相同，但是INSTANCE\_NAME不同
4.  DB\_NAME还在动态注册监听的时候起作用，无论是否定义了SERVICE\_NAME,PMON进程都会使用DB\_NAME动态注册监听

## DBID

1.  DBID可以看做是DB\_NAME在数据库内部的表示，它是在数据库创建的时候用DB\_NAME结合算法计算出来的
2.  它存在于datafile和control file中，用来表示数据文件的归属，所以DBID是唯一的，对于不同的数据库，DB\_NAME可以是相同的，但是DBID一定是唯一的，例如在DataGuard中，主备库的DB\_NAME相同，但是DBID一定不同（看过一个很形象的例子，就是可以有同名的人，但是身份证号码一定不同）

## DB\_UNIQUE\_NAME

1.  在DataGuard中，主备库拥有相同的DB\_NAME，为了区别，就必须有不同的DB\_UNIQUE\_NAME
2.  DB\_UNIQUE\_NAME在DG中会影响动态注册的SERVICE\_NAME，即如果采用的是动态注册，则注册的SERVICE\_NAME为DB\_UNIQUE\_NAME，但是实例还是INSTANCE\_NAME，即SID

## INSTANCE\_NAME

1.  数据库实例的名称，INSTANCE\_NAME默认值是SID，一般情况下和数据库名称（DB\_NAME)相同，也可不同
2.  initSID.ora 和orapwSID 文件要与INSTANCE\_NAME保持一致
3.  INSTANCE\_NAME会影响进程的名称

## SID

创建：在dbca时确定

1.  是操作系统中的环境变量，和ORACLE\_HOME,ORACLE\_BASE用法相同
2.  在操作系统中要想得到实例名，就必须使用ORACLE\_SID。且ORACLE\_SID必须与INSTANCE\_NAME的值一致

## SERVICE\_NAME

1.  数据库和客户端相连是使用的服务名
2.  在DataGuard中，如果采用动态注册，建议在主备库使用相同的service\_names
3.  在DataGuard中，如果采用静态注册，建议在主备库上的listener中输入相同的服务名(service\_name)
4.  如果采监听采用了静态注册，那么SERVICE\_NAME就等于Listener.ora 文件中的GLOBAL\_DATABASE\_NAME的值

## GLOBAL\_DATABASE\_NAME

1.  GLOBAL\_DATABASE\_NAME 是listener配置的对外网络连接名称，可以是任意值
2.  在客户端配置监听的tnsnames.ora 文件中的service\_name 与这个GLOBAL\_DBNAME 保持一致就可以了
3.  配置静态监听注册时，需要输入SID和GLOBAL\_NAME
