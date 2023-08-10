# icmp

允许请求进来，临时升效

```纯文本
iptables  -A INPUT -p icmp --icmp 8 -j ACCEPT
```

允许响应出去，临时升效

```纯文本
iptables -A OUTPUT  -p icmp --icmp 0 -j ACCEPT
```

> type 8为ping 命令请求信号
> type 0为ping 命令响应信号

保存到规则文件

```纯文本
service iptables save
```
