## 删除

```
dbca -silent -deleteDatabase -sourceDB orcl -sysDBAPassword Sino_ZJ001
```

删除旧的数据文件

```
rm -rf $ORACLE_BASE/oradata/$ORACLE_SID
```

