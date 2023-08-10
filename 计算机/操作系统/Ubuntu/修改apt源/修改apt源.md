# 修改apt源

## 目录

-   [18.04（bionic）](#1804bionic)
-   [20.04（focal）](#2004focal)
-   [21.04（hirsute）](#2104hirsute)

查看代号：lsb\_release -c

修改文件：/etc/apt/source.list

### 18.04（bionic）

```纯文本
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
```

### 20.04（focal）

```bash
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
```

### 21.04（hirsute）

```纯文本
deb http://mirrors.aliyun.com/ubuntu/ hirsute main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ hirsute main restricted
```
