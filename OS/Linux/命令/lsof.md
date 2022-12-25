查看LISTEN的TCP端口的所有进程

```
lsof -i tcp -sTCP:LISTEN -P
```

根据端口查看进程

```
lsof -i:16824
```

