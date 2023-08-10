# 创建CDB、可插拔数据库

## 目录

-   [从dbca创建](#从dbca创建)

## 从dbca创建

CDB

```纯文本
dbca -silent -createdatabase -templateName General_Purpose.dbc -responseFile NO_VALUE \
-gdbname CDB -sid CDB \
-characterSet ZHS16GBK -nationalCharacterSet AL16UTF16 \
-sysPassword password -systemPassword password \
-emConfiguration LOCAL -createAsContainerDatabase true

```

可插拔数据库，同时创建了adminplug公共用户

```纯文本
dbca -silent -createPluggableDatabase -sourceDB CDB2 -pdbName mypdb \
-pdbAdminUserName adminplug -pdbAdminPassword foo \
-pdbDatafileDestination /u01/dbfile/CDB/mypdb \
-createPDBFrom DEFAULT

```
