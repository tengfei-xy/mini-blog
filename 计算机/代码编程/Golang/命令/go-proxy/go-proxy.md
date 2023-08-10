# go-proxy

## 目录

-   [windows](#windows)
-   [linux/mac](#linuxmac)
-   [源](#源)

# windows

```纯文本
set GOPROXY=https://mirrors.aliyun.com/goproxy/
```

使用socks5代理

```纯文本
set http_proxy=socks5://192.168.0.4:1091
set https_proxy=socks5://192.168.0.4:1091
```

# linux/mac

使用第三方代理

```纯文本
# 使用第三方代理
export GOPROXY="https://mirrors.aliyun.com/goproxy/,direct"
# 恢复默认
export GOPROXY="direct"
```

直接写入

```纯文本
go env -w GOPROXY=https://proxy.golang.com.cn,direct
```

使用系统代理

```纯文本
export http_proxy=socks5://127.0.0.1:57204
export https_proxy=socks5://127.0.0.1:57204
```

# 源

```纯文本
https://proxy.golang.com.cn
```
