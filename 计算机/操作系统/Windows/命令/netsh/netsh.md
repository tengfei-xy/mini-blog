# netsh

## 目录

-   [简写](#简写)
-   [ip](#ip)
-   [DNS](#DNS)
-   [网卡](#网卡)
-   [winhttp](#winhttp)
-   [有效名称策略表](#有效名称策略表)

## 简写

详写

```纯文本
netsh interface show interface
```

下方命令等同于上方

```纯文本
netsh i s i
```

## ip

修改IP

```纯文本
netsh interface ipv4 set address "以太网 2" static 192.168.1.98 255.255.255.0 192.168.1.1
```

查看是否启动DHCP，其中7为网卡索引

```纯文本
netsh i i sh a 7
```

自动获取IP

```纯文本
netsh i i se ad 11 dh
```

## DNS

修改主要DNS

```纯文本
netsh interface ip set dns "本地连接" static 172.16.41.130 primary
```

添加次要DNS

```纯文本
netsh interface ip add dnsservers "本地连接" 8.8.8.8 index=2
```

自动获取DNS

```纯文本
netsh i i se dn 11 dh
```

## 网卡

查看正在使用的接口

```纯文本
netsh interface show interface
netsh i sh i
```

查看被禁用的接口

```纯文本
netsh i sh i
```

启用（e表示admin=enabeld）

```纯文本
netsh i se i "网卡名" e
```

禁用网卡（d表示admin=disabled）

```纯文本
netsh i se i "网卡名" d
```

## winhttp

查看winhttp代理，系统代理可能会造成winhttp也被设置，但其实有区别，[winhttp代理相关论坛](https://social.technet.microsoft.com/Forums/windows/en-US/baf4f5bf-1214-4348-8dea-201dcafd488c/the-winrm-client-received-an-http-status-code-of-502-from-the-remote-wsmanagement-service?forum=exchange2010 "winhttp代理相关论坛")

```bash
 netsh winhttp show proxy
```

清除winhttp代理

```bash
 netsh winhttp reset proxy
```

添加http代理

```纯文本
netsh winhttp set proxy 127.0.0.1:51630
```

## 有效名称策略表

```纯文本
netsh name sh effectivepolicy
```
