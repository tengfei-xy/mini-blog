## websocket 反向代理

```
location /baidu{
        proxy_pass                  http://116.62.78.44:1765;
        proxy_set_header            Upgrade $http_upgrade;
        proxy_set_header            Connection "Upgrade";
}
```

