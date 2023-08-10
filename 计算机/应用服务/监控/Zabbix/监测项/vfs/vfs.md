# vfs

## 目录

-   [vfs.fs.size](#vfsfssize)

官方文档：[https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix\_agent](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix_agent "https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix_agent")

## vfs.fs.size

说明：以字节为单位的磁盘空间或占总空间的百分比。

格式

```纯文本
vfs.fs.size[fs,<mode>]
```

参数说明

```纯文本
fs - filesystem
mode - possible values:
total (default), free, used, pfree (free, percentage), pused (used, percentage)
```

返回值

```纯文本
整数 - 字节
浮点数 - 百分比
```

示例

```纯文本
# / 的空余百分比
vfs.fs.size[/,pfree]
```
