# lsof

查看LISTEN的TCP端口的所有进程

```纯文本
lsof -i tcp -sTCP:LISTEN -P
```

根据端口查看进程

```纯文本
lsof -i:16824
```

查看程序打开的文件数

```纯文本
lsof -p PID | wc -l
```
