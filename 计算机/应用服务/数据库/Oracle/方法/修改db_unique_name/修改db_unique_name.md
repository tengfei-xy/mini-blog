# 修改db\_unique\_name

```纯文本
SQL> alter system set db_unique_name=primarydb scope=spfile;
SQL> startup force;
SQL> show parameter db_unique_name
```
