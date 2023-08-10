# expimp

## 目录

-   [exp/imp](#expimp)
    -   [其他参数](#其他参数)

# exp/imp

<https://cloud.tencent.com/developer/news/285439>

注：用户名需要大写，否则会被当成参数

exp SYSTEM/password file=exp.dmp full=y

imp SYSTEM/password file=exp2.dmp full=y

## 其他参数

忽略： `ignore=y`默认n

导出数据行：`rows=y`，默认y，否则导出结构

OWNER：指定用户

TABLES：指定表名
