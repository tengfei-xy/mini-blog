官方文档：https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/simple_checks#icmp-pings

icmpping、icmppingLost、icmppingsec依赖于fping，安装于zabbix_server。

```bash
wget http://fping.org/dist/fping-3.15.tar.gz
tar xf fping-3.15.tar.gz && cd fping-3.15
./configure --prefix=/usr && make -j4 && make install
chown root:zabbix  /usr/sbin/fping && chmod 4710 /usr/sbin/fping
# zabbix 的登录shell保持/sbin/nologin
```

> fping的安装路径注意：zabbix_server读取配置源于zabbix_server.conf的FpingLocation来作为fping的安装位置，默认是/usr/sbin/fping，因此配置时`./configure --prefix=/usr`
>
> 当zabbix_server.conf中的DebugLevel>=4时才能看到fping的调用过程

> 键值使用注意：该三者键依赖于server（表示需要从网页端设置），无法从zabbix_get或zabbix客户端或zabbix客户端（主动式）获取icmp的结果，否则报错 Invalid first parameter.



## icmpping

说明：通过 ICMP ping 的主机可访问性。

格式

```
icmpping[<target>,<packets>,<interval>,<size>,<timeout>]
```

参数说明

```
target - 主机 IP 或 DNS 名称
packets - 数据包数量
interval - 连续数据包之间的时间（以毫秒为单位）
size - 数据包大小（以字节为单位）
timeout - 超时（以毫秒为单位）
```

返回值

```
0 - ICMP ping 失败
1 - ICMP ping 成功
```

常用指标

- 四个ping包任意掉一个返回0

  ```
  icmpping[,4]
  ```

  



## icpmpinglost

说明：丢失数据包的百分比。

格式

```
icmpping[<target>,<packets>,<interval>,<size>,<timeout>]
```

返回：浮点值



## 参数说明

| Parameter | Unit         | Description                                                  | Fping's flag | Defaults-fping                                               | Defaults-Zabbix | Zabbix-min | **Zabbix-max** |
| :-------- | :----------- | :----------------------------------------------------------- | :----------- | :----------------------------------------------------------- | :-------------- | ---------- | -------------- |
| packets   | number       | number of request packets to a target                        | -C           |                                                              | 3               | 1          | 10000          |
| interval  | milliseconds | time to wait between successive packets                      | -p           | 1000                                                         |                 | 20         | unlimited      |
| size      | bytes        | packet size in bytes 56 bytes on x86, 68 bytes on x86_64     | -b           | 56 or 68                                                     |                 | 24         | 65507          |
| timeout   | milliseconds | **fping v3.x** - timeout to wait after last packet sent, affected by *-C* flag **fping v4.x** - individual timeout for each packet | -t           | **fping v3.x** - 500 **fping v4.x** - inherited from *-p*flag, but not more than 2000 |                 | 50         | unlimited      |
