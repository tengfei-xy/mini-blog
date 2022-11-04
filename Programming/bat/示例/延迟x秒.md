# 延迟 x秒方法

方法1

```
TIMEOUT >nul 2>nul /T 2
```

方法2

```
ping 127.0.0.1 -n 1>nul
```

