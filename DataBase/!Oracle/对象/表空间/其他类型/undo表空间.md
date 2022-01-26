# 查看

查看当前实例使用的表空间

```sql
Show parameter undo_tablespace
```



显示数据库的所有UNDO表空间

```sql
SELECT tablespace_name FROM dba_tablespaces WHERE contents='UNDO';
```



## 创建

创建undo 类型

`create undo tablespace undo1 datafile '/ora10/product/oradata/ora10/paul01.dbf' size 20m;`

