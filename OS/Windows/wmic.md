# wmic
获取名称
`wmic CPU get name`

获取主机名
`wmic computersystem get name`

获取进程所在目录
`wmic process get caption,executablepath`

远程执行程序（无需ADMIN$）

`wmic /node:192.168.8.20 /user:administrator /password:804
870230 process call create "cmd.exe /c calc.exe"`

获取网速（BytesReceivedPersec  BytesSentPersec  BytesTotalPersec  Caption）

`where "name like '%eth%'" get BytesTotalPersec|findstr "\<[0-9]"`