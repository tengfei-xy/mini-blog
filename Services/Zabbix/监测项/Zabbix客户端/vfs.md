官方文档：https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix_agent

## vfs.fs.size

说明：以字节为单位的磁盘空间或占总空间的百分比。

格式

```
vfs.fs.size[fs,<mode>]
```

参数说明

```
fs - filesystem
mode - possible values:
total (default), free, used, pfree (free, percentage), pused (used, percentage)
```

返回值

```
整数 - 字节
浮点数 - 百分比
```



示例

```
# / 的空余百分比
vfs.fs.size[/,pfree]
```

