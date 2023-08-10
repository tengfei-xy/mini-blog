# 基本SQL

// informix 查看数据库名
onstat -c | grep DBSERVER

// informix 执行数据导出
unload to /opt/informix/abc.csv delimiter "," select \* from test;

// informix 导出表结构
dbschema -d test -t all db.sql

// informix 使用命令行的方式执行 test.sql
dbaccess test test.sql

// informix 获取database下的表
dbschema -d test -t all | grep TABLE | awk '{print \$3}' | cut -d . -f 2

// informix 获取用户下的所有database
select name from sysmaster:sysdatabases;

//informix 获取用户创建的所有database
select name from sysdatabases where name not in ('sysmaster','sysutils','sysuser','sysadmin','sysha');

// informix 获取test这个database下的表
select dbsname,tabname from sysmaster:systabnames where dbsname='test';

// informix 获取所有database下的表
select \* from sysmaster:systabnames ;

// informix 获取当前连接的database下用户自己创建的表
select \* from systables where tabtype='T' and tabid>99;

// informix 获取所有的表
select \* from systables

// informix 给test表添加num字段，类型为integer
alter table test add num integer

// informix 创建唯一索引（其中test为表名 ， age为需要创建的字段）
alter table test modify age integer primary key ;

// informix 查找外键，其中constrtype 为R
select constrname from sysconstraints where constrtype='R' and tabid= ( select tabid from systables where tabname = 'test' ) ;

// informix 查找主键，其中constrtype 为P
select constrname from sysconstraints where constrtype='P' and tabid= ( select tabid from systables where tabname = 'test' );
// 如查找tabid为103的表
select \* from sysconstraints where tabid=103;

// informix 删除主键
alter table tablename drop constraint constrname;
//如删除名为 u103\_13 的主键：
alter table test drop constraint u103\_13;
