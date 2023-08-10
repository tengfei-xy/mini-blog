# mac管理服务

建议使用launchcontrol(Global Daemons)管理/Library/LaunchDaemons/homebrew\.mxcl.dnsmasq.plist服务

启动

```纯文本
/usr/local/opt/dnsmasq/sbin/dnsmasq --keep-in-foreground -C /usr/local/etc/dnsmasq.conf -7 /usr/local/etc/dnsmasq.d,*.conf
```

测试配置文件

```纯文本
/usr/local/opt/dnsmasq/sbin/dnsmasq --test
```
