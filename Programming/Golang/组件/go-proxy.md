# windows

```
set GOPROXY=https://mirrors.aliyun.com/goproxy/
```

使用socks5代理

```
set http_proxy=socks5://192.168.0.4:1091
set https_proxy=socks5://192.168.0.4:1091
```

# linux/mac

```
export GOPROXY="https://mirrors.aliyun.com/goproxy/,direct"
```

直接写入

```
go env -w GOPROXY=https://proxy.golang.com.cn,direct
```

# 源

```
https://proxy.golang.com.cn
```



