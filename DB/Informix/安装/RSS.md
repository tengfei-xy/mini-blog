## 基本信息

注：服务器初始化后开始rss

| DBSERVERNAME | hostname  | server type |
| :----------: | :-------: | :---------: |
|     rss1     | host_rss1 |   Primary   |
|     rss2     | host_rss2 |     RSS     |

[参考链接](https://blog.csdn.net/marvelyu/article/details/7061676)

## 主从服务器同时修改


修改 $INFORMIXDIR/etc/onconfig.std

```
TAPEDEV STDIO
LOG_INDEX_BUILDS 1
LTAPEDEV /dev/null
```
修改 $INFORMIXDIR/etc/sqlhosts

```
rss1 onsoctcp host_rss1 ifx_service
rss2 onsoctcp host_rss2 ifx_service
```

修改 信任远程主机,[14.10参考说明](https://www.ibm.com/docs/en/informix-servers/14.10/14.10?topic=parameters-remote-users-cfg-configuration-parameter)

```
echo `hostname` > /etc/host.rhost
```

关闭

```
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

```
onstat -g rss
```