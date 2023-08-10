# wmic

> 注：WMI 命令行 (WMIC) 实用程序在 Windows 服务器的21H1 半年通道版本中已弃用。其代替品是Get-wmiobject

获取CPU名称

```纯文本
wmic CPU get name
```

获取CPU所有信息

```纯文本
wmic CPU list full
```

获取CPU核心数

```纯文本
wmic CPU get NumberOfLogicalProcessors
```

获取CPU线程数

```纯文本
wmic cpu get NumberOfCores
```

内存最大内存数

```纯文本
wmic memphysical list brief
```

获取主机名
`wmic computersystem get name`

获取进程所在目录
`wmic process get caption,executablepath`

远程执行程序（无需ADMIN\$）

`wmic /node:192.168.8.20 /user:administrator /password:804
870230 process call create "cmd.exe /c calc.exe"`

获取网速（BytesReceivedPersec  BytesSentPersec  BytesTotalPersec  Caption）

`where "name like '%eth%'" get BytesTotalPersec|findstr "\<[0-9]"`
