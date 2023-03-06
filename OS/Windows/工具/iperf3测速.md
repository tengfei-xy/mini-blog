# iperf3 测速

下载地址：http://software.es.net/iperf/news.html#iperf-3-11-released

## 参数
-s server
-c client + server IP
-u udp
-b bandwidth，这个是用来表示使用多大带宽进行发包，根据真实环境进行调整
-t time，发包多长时间，单位是秒
-i interval，结果输出间隔时间，如果不加这个参数，那么就等到全部测试完成才给结果

## 方法
客户端向服务端发送数据
服务端：`iperf3 -s`
客户端：`iperf3 -c  ip -b 1000M -t 30 -i 5`

