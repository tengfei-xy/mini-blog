## 简写

详写

```
netsh interface show interface
```

下方命令等同于上方

```
netsh i s i
```



## 例

修改主要DNS

```
netsh interface ip set dns "本地连接" static 172.16.41.130 primary
```

添加次要DNS

```
netsh interface ip add dnsservers "666" 8.8.8.8 index=2
```

修改IP

```
netsh interface ipv4 set address "以太网 2" static 192.168.1.98 255.255.255.0 192.168.1.1
```

查看正在使用的interface接口

```
netsh interface show interface
```

查看winhttpd代理，系统代理可能会造成winhttp也被设置，但其实有区别，[winhttp代理相关论坛](https://social.technet.microsoft.com/Forums/windows/en-US/baf4f5bf-1214-4348-8dea-201dcafd488c/the-winrm-client-received-an-http-status-code-of-502-from-the-remote-wsmanagement-service?forum=exchange2010)

```bash
 netsh winhttp show proxy
```

清除winhttp代理

```bash
 netsh winhttp reset proxy
```

