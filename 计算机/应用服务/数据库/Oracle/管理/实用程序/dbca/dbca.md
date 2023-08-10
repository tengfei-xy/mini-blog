# dbca

## 目录

-   [删除](#删除)

## 删除

```纯文本
dbca -silent -deleteDatabase -sourceDB orcl -sysDBAPassword Sino_ZJ001
```

删除旧的数据文件

```纯文本
rm -rf $ORACLE_BASE/oradata/$ORACLE_SID
```
