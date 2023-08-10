# 虚拟VIP

## 目录

-   [临时方法](#临时方法)
-   [永久方法](#永久方法)

## 临时方法

```bash
$ ifconfig <当前网卡名>:0 <virtual_ip> netmask <virtual_mask> up
```

## 永久方法

```bash
cd /etc/sysconfig/network-scripts
cp ifcfg-ens33 ifcfg-ens33:0
vi ifcfg-ens33:0

DEVICE=ens33:0
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.244.2
NETMASK=255.255.255.0
GATEWAY=192.168.244.1
HWADDR=00:0c:29:52:78:61
USERCTL=no

systemctl restart network
```
