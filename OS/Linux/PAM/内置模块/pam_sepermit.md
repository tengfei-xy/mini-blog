在SElinux处于强制模式才能访问。当禁用SELinux时，将返回PAM_IGNORE（忽略该模块）

对应的默认配置文件为/etc/security/sepermit.conf，格式为

```
user[:group:option]
```

