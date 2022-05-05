# ssh代理

[参考链接](https://zhuanlan.zhihu.com/p/57630633)

## sock5代理

命令：`ssh -D 本地监听地址 远程主机IP`

优化命令：`ssh -CqTnN -L 0.0.0.0:PortA:HostC:PortC 远程主机IP `

优化命令解释:其中 `-C` 为压缩数据，`-q` 安静模式，`-T` 禁止远程分配终端，`-n` 关闭标准输入，`-N` 不执行远程命令。此外视需要还可以增加 `-f` 参数，把 ssh 放到后台运行。

## 使用代理访问目标主机

```bash
ssh -o ProxyCommand="nc -X 5 -x 127.0.0.1:1091 %h %p"
```

