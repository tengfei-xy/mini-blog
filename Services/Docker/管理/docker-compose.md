# 安装 Docker-compose

下载[Docker-compose](https://github.com/docker/compose/releases)

```bash
wget https://github.com/docker/compose/releases/download/v2.17.0-rc.1/docker-compose-linux-x86_64
```

移动到PATH

```bash
mv ./docker-compose-linux-x86_64 /usr/local/bin
ln -s /usr/local/bin/docker-compose-linux-x86_64 /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose-linux-x86_64
```
