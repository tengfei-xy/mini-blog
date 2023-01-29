# 客户端设置

修改/etc/dhcpcd.conf

1. 把duid改成dhcpcd
2. 添加interface ens33，作为使用dhcp客户端的接口
3. 重启服务，`systemctl restart dhcpcd`
4. 开机自启服务,`systemctl enable dhcpcd`

## dhcp设置静态IP

编辑/etc/dhcpcd.conf

```
interface ethX
static ip_address=192.168.xxx.xxx/24 #这里用CIDR的格式配置地址
static routers=192.168.xxx.xxx #这里配置的是网关
static domain_name_servers=192.168.xxx.xxx #这里配置域名服务器地址
```



## 其他

问题：ip命令重启后失效

> [iproute2](https://en.wikipedia.org/wiki/iproute2) 是 [base](https://archlinux.org/packages/?name=base)包 [元包](https://wiki.archlinuxcn.org/wzh/index.php?title=Meta_package&action=edit&redlink=1) 的依赖，提供 [ip(8)](https://man.archlinux.org/man/ip.8) 命令行接口，用于管理 [网络接口](https://wiki.archlinuxcn.org/wiki/网络配置#网络接口)，[IP 地址](https://wiki.archlinuxcn.org/wiki/网络配置#IP_地址) 和 [路由表](https://wiki.archlinuxcn.org/wiki/网络配置#路由表)。注意使用 `ip` 进行的配置会在重启后丢失。要进行永久配置，可以使用 [网络管理器](https://wiki.archlinuxcn.org/wiki/网络配置#网络管理器) 或通过脚本和 [systemd 单元](https://wiki.archlinuxcn.org/wiki/Systemd#编写单元文件) 使 *ip* 命令自动化。

解决方法：

编辑/usr/lib/systemd/system/dhcpcd.service，添加下列内容到service模块下

```
ExecStartPre=ip link set ens33 up
```

重启服务

```
systemctl daemon-reload
systemctl restart dhcpcd
```

