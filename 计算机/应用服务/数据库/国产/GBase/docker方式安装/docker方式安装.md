# docker方式安装

镜像下载

```bash
docker pull shihd/gbase8a:1.0 
```

镜像运行

```bash
docker run --name gbase -it -p5258:5258 shihd/gbase8a:1.0
```

镜像作为后台进程运行

```纯文本
docker run --name gbase -d -p5258:5258 shihd/gbase8a:1.0
```

**GBase8a数据库信息**

DB: gbase

User: root

Password: root

Port: 5258

192.168.3.54:5258
