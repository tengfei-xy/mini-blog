## 在集群中创建一个MySQL租户

bootstrap之后, 默认创建的mysql租户名称是 `mysql`, 如果你要自己创建租户, 请参考OceanBase运维手册或者OCP产品帮助文档创建一个MySQL租户。  

## 执行示例数据库创建脚本

登录mysql租户的sys用户，执行脚本`mysql_example/create_mysql_example.sql`。


示例如下：

```bash
./hap.py ob1.mysqlmode < mysql_example/create_mysql_example.sql
```

执行成功后，会按照tpcc的schema创建表, 并导入一些数据。

```bash
Tablegroup_name Table_name      Database_name
oceanbase       NULL    NULL
tpcc_group      NULL    NULL
Grants for tpcc@%
GRANT USAGE ON *.* TO 'tpcc'
GRANT SELECT ON `oceanbase`.* TO 'tpcc'
GRANT ALL PRIVILEGES ON `tpccdb`.* TO 'tpcc'
Tables_in_tpccdb
cust
dist
hist
item
load_hist
load_proc
nord
ordl
ordr
stock_item
stok
ware
table_name      rows_cnt
WARE    2
DIST    20
NORD    40
ORDR    60
HIST    240
ITEM    622
ORDL    626
CUST    1040
STOK    1244
Tables_in_tpccdb
cust
dist
hist
item
load_hist
load_proc
nord
ordl
ordr
stock_item
stok
ware
```

脚本会创建一个业务用户：`tpcc`，密码是：`123456`。登陆方式如下：

```bash
./hap.py ob1.obs0.mysqlmode_tpcc

```

##  删除示例数据库脚本(可选)

```bash
./hap.py ob1.obs0.mysqlmode <mysql_example/drop_mysql_sample.sql
```