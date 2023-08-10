# RSS

## 目录

-   [基本信息](#基本信息)
-   [主从服务器同时修改](#主从服务器同时修改)
-   [RSS节点操作](#RSS节点操作)
-   [检查](#检查)

## 基本信息

注：服务器初始化后开始rss

| DBSERVERNAME | hostname   | server type |
| ------------ | ---------- | ----------- |
| rss1         | host\_rss1 | Primary     |
| rss2         | host\_rss2 | RSS         |

<https://blog.csdn.net/marvelyu/article/details/7061676>

## 主从服务器同时修改

修改 \$INFORMIXDIR/etc/onconfig.std

```纯文本
TAPEDEV STDIO
LOG_INDEX_BUILDS 1
LTAPEDEV /dev/null
```

修改 \$INFORMIXDIR/etc/sqlhosts

```纯文本
rss1 onsoctcp host_rss1 ifx_service
rss2 onsoctcp host_rss2 ifx_service
```

修改 信任远程主机,[14.10参考说明](https://www.ibm.com/docs/en/informix-servers/14.10/14.10?topic=parameters-remote-users-cfg-configuration-parameter "14.10参考说明")

```纯文本
echo `hostname` > /etc/host.rhost
```

关闭

```纯文本
onmode -ky
```

## RSS节点操作

主服务器操作：

```shell
# 启动
oninit -vy

# 添加节点
onmode -d add RSS rss2

# 执行这条命令时，从服务器的online.log会有日志输出
ontape -s -L 0 -F | rsh host_rss2 ". ~/.bash_profile;ontape -p"
```

从服务器上操作

```shell
# 启动
oninit -r

# 切换到RSS模式
onmode -d RSS rss1
```

## 检查

查看 RSS 节点状态

```纯文本
onstat -g rss
```
