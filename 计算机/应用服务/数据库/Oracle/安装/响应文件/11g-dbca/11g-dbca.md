# 11g-dbca

命令行式，暂有问题

```纯文本
dbca -silent \
-createDatabase \
-responseFile NO_VALUE \
-templateName General_Purpose.dbc \
-gdbName rac3 \
-sid rac3 \
-systemPassword Password@1 \
-sysPassword Password@1 \
-emConfiguration NONE \
-datafileDestination +DATA \
-recoveryAreaDestination +DATA \
-storageType ASM \
-characterSet AL32UTF \
-nationalCharacterSet AL32UTF \
-nodelist ora1,ora2 \
-sampleSchema false \
-databaseType OLTP
```
