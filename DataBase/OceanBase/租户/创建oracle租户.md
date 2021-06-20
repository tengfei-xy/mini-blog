[其他参考链接](https://www.modb.pro/db/29022)

# 手动创建租户

查看可用资源

```sql
select
	zone,
	sum(cpu_total) - sum(cpu_max_assigned) as 'CPU remaining C',
	(sum(mem_total) - sum(mem_max_assigned))/ 1024 / 1024 / 1024 as 'Memory remaining G'
from __all_virtual_server_stat
group by zone;
```

定义资源规则

```sql
-- 创建
create resource unit unit_01 max_cpu=2, min_cpu=1, max_memory='2G', min_memory='1G', max_iops=10000, min_iops=6000, max_session_num=5000, max_disk_size='30G';
-- 查询
select * from oceanbase.`__all_unit_config`
-- 修改
ALTER RESOURCE UNIT unit_config_01 max_cpu=3;
-- 删除
drop resource unit unit_config_01;
```

创建资源池

```sql
create resource pool pool_01 unit=unit_config_01, unit_num=1; 
```





# 通过试例脚本创建租户



## 在集群中创建一个ORACLE租户

bootstrap之后, 默认创建的oracle租户名称是 `oracle`, 如果你要自己创建租户, 请参考OceanBase运维手册或者OCP产品帮助文档创建一个Oracle租户。  

## 执行示例数据库创建脚本

登录oracle租户的sys用户，执行脚本`oracle_example/create_oracle_example.sql`。

示例如下：

```bash
./hap.py ob1.oraclemode < oracle_example/create_oracle_example.sql
```

执行成功后，会按照tpcc的schema创建表, 并导入一些数据。

```bash
TABLE_NAME      ROWS_CNT
WARE    2
DIST    20
NORD    40
ORDR    60
HIST    240
ITEM    622
ORDL    626
CUST    1040
STOK    1244
OWNER   OBJECT_NAME     OBJECT_TYPE     STATUS
TPCC    __idx_1100611139453780_ICUST    INDEX   VALID
TPCC    __idx_1100611139453784_IORDR    INDEX   VALID
TPCC    DELIVERY        PROCEDURE       VALID
TPCC    NEWORDER        PROCEDURE       VALID
TPCC    ORDERSTATUS     PROCEDURE       VALID
TPCC    PAYMENT PROCEDURE       VALID
TPCC    STOCKLEVEL      PROCEDURE       VALID
TPCC    CUST    TABLE   VALID
TPCC    DIST    TABLE   VALID
TPCC    HIST    TABLE   VALID
TPCC    ITEM    TABLE   VALID
TPCC    LOAD_HIST       TABLE   VALID
TPCC    LOAD_PROC       TABLE   VALID
TPCC    NORD    TABLE   VALID
TPCC    ORDL    TABLE   VALID
TPCC    ORDR    TABLE   VALID
TPCC    STOK    TABLE   VALID
TPCC    WARE    TABLE   VALID
TPCC    CUST    TABLE PARTITION VALID
TPCC    DIST    TABLE PARTITION VALID
TPCC    HIST    TABLE PARTITION VALID
TPCC    NORD    TABLE PARTITION VALID
TPCC    ORDL    TABLE PARTITION VALID
TPCC    ORDR    TABLE PARTITION VALID
TPCC    STOK    TABLE PARTITION VALID
TPCC    WARE    TABLE PARTITION VALID
TPCC    STOCK_ITEM      VIEW    VALID
```

脚本会创建一个业务用户：`tpcc`，密码是：`123456`。登陆方式如下：

```bash
./hap.py ob1.oraclemode_tpcc
```

## 删除示例数据库脚本(可选)

```bash
./hap.py ob1.oraclemode < oracle_example/drop_oracle_sample.sql
```
成功后提示