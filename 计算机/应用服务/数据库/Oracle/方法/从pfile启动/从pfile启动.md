# 从pfile启动

启动

```纯文本
SQL> startup nomount pfile='/data/app/oracle/product/11.2.0/db_1/dbs/initoracle.ora'
```

保存

```纯文本
SQL> create spfile from pfile='/data/app/oracle/product/11.2.0/db_1/dbs/initoracle.ora'
```

启动

```纯文本
startup force
```
