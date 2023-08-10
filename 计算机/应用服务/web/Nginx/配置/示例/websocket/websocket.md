# websocket

## 目录

-   [websocket 反向代理](#websocket-反向代理)

## websocket 反向代理

```nginx
map $http_upgrade $connection_upgrade { 
    default          keep-alive;  #默认为keep-alive 可以支持 一般http请求
    'websocket'      upgrade;     #如果为websocket 则为 upgrade 可升级的。
}

server {
    location /chat/ {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade; #此处配置 上面定义的变量
        proxy_set_header Connection $connection_upgrade;
    }
}
```

```nginx
location /baidu{
        proxy_pass                  http://116.62.78.44:1765;
        proxy_set_header            Upgrade $http_upgrade;
        proxy_set_header            Connection "Upgrade";
}
```
