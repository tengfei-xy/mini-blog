查看集群状态

 ```
 crsctl stat res -t
 ```

检查进程

```
ps -ef|grep ctss
```

集群验证

```
crsctl check ctss
cluvfy comp clocksync -n all -verbose
```

 全部启动

```
crsctl start cluster -all
```



 crsctl start res ora.ctssd -init 

 crsctl stop res ora.ctssd -init

crsctl stat res -t -init

