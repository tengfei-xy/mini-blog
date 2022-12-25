## DB_NAME

创建：在dbca时确定

1. 是数据库名，长度不能超过8个字符，记录在datafile、redolog和control file中

2. 在DataGuard环境中DB_NAME相同而DB_UNIQUE_NAME不同

3. 在RAC环境中，各个节点的DB_NAME 都相同，但是INSTANCE_NAME不同

4. DB_NAME还在动态注册监听的时候起作用，无论是否定义了SERVICE_NAME,PMON进程都会使用DB_NAME动态注册监听



## DBID

1. DBID可以看做是DB_NAME在数据库内部的表示，它是在数据库创建的时候用DB_NAME结合算法计算出来的

2. 它存在于datafile和control file中，用来表示数据文件的归属，所以DBID是唯一的，对于不同的数据库，DB_NAME可以是相同的，但是DBID一定是唯一的，例如在DataGuard中，主备库的DB_NAME相同，但是DBID一定不同（看过一个很形象的例子，就是可以有同名的人，但是身份证号码一定不同）



## DB_UNIQUE_NAME

1. 在DataGuard中，主备库拥有相同的DB_NAME，为了区别，就必须有不同的DB_UNIQUE_NAME

2. DB_UNIQUE_NAME在DG中会影响动态注册的SERVICE_NAME，即如果采用的是动态注册，则注册的SERVICE_NAME为DB_UNIQUE_NAME，但是实例还是INSTANCE_NAME，即SID



## INSTANCE_NAME

1. 数据库实例的名称，INSTANCE_NAME默认值是SID，一般情况下和数据库名称（DB_NAME)相同，也可不同

2. initSID.ora 和orapwSID 文件要与INSTANCE_NAME保持一致

3. INSTANCE_NAME会影响进程的名称

 

## SID

创建：在dbca时确定

1. 是操作系统中的环境变量，和ORACLE_HOME,ORACLE_BASE用法相同

2. 在操作系统中要想得到实例名，就必须使用ORACLE_SID。且ORACLE_SID必须与INSTANCE_NAME的值一致

 

## SERVICE_NAME

1. 数据库和客户端相连是使用的服务名

2. 在DataGuard中，如果采用动态注册，建议在主备库使用相同的service_names

3. 在DataGuard中，如果采用静态注册，建议在主备库上的listener中输入相同的服务名(service_name)

4. 如果采监听采用了静态注册，那么SERVICE_NAME就等于Listener.ora 文件中的GLOBAL_DATABASE_NAME的值

 

## GLOBAL_DATABASE_NAME

1. GLOBAL_DATABASE_NAME 是listener配置的对外网络连接名称，可以是任意值

2. 在客户端配置监听的tnsnames.ora 文件中的service_name 与这个GLOBAL_DBNAME 保持一致就可以了

3. 配置静态监听注册时，需要输入SID和GLOBAL_NAME