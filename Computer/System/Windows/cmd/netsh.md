# netsh
## 例子

修改主要DNS：netsh interface ip set dns "本地连接" static 172.16.41.130 primary
添加次要DNS：netsh interface ip add dnsservers "666" 8.8.8.8 index=2
修改IP：netsh interface ipv4 set address "以太网 2" static 192.168.1.98 255.255.255.0 192.168.1.1
查看正在使用的interface接口
netsh interface show interface

## 参数说明

netsh interface ip show /?
show ipaddresses - 显示当前 IP 地址。
show addresses - 显示 IP 地址配置。
show ipstats   - 显示 IP 统计。
show compartments - 显示分段参数。
show config    - 显示 IP 地址和其他信息。
show destinationcache - 显示目标缓存项目。
show dnsservers - 显示 DNS 服务器地址。
show dynamicportrange - 显示动态端口范围配置参数。
show global    - 显示全局配置普通参数。
show icmpstats - 显示 ICMP 统计。
show interfaces - 显示接口参数。
show ipnettomedia - 显示 IP 的网络到媒体的映射。
show joins     - 显示加入的多播组。
show neighbors - 显示邻居缓存项。
show offload   - 显示卸载信息。
show route     - 显示路由表项目。
show subinterfaces - 显示子接口参数。
show tcpconnections - 显示 TCP 连接。
show tcpstats  - 显示 TCP 统计。
show udpconnections - 显示 UDP 连接。
show udpstats  - 显示 UDP 统计。
show winsservers - 显示 WINS 服务器地址。