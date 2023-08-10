# crsctl

查看集群状态

```纯文本
crsctl stat res -t
```

检查进程

```纯文本
ps -ef|grep ctss
```

集群验证

```纯文本
crsctl check ctss
cluvfy comp clocksync -n all -verbose
```

全部启动

```纯文本
crsctl start cluster -all
```

crsctl start res ora.ctssd -init

crsctl stop res ora.ctssd -init

crsctl stat res -t -init
