# Centos9yum安装7.4

```纯文本
sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-9.rpm
sudo yum install -y yum-utils
sudo yum-config-manager --enable remi-php74
sudo yum update -y
sudo yum install -y php php-cli php-fpm php-mysqlnd php-zip php-devel php-gd php-mbstring php-curl php-xml php-pear php-bcmath
```

启动

```纯文本
sudo systemctl start php-fpm
```

开机自启

```纯文本
sudo systemctl enable php-fpm
```
