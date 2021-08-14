[参考链接](https://blog.csdn.net/chexlong/article/details/102581063)

```
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.27.tar.gz
yum install mysql-community-client-plugins-8.0.24-1.el7.x86_64
yum install mysql-community-{client,common,devel,embedded,libs,server}-*
```

查看初始密码

```
grep "password" /var/log/mysqld.log
```

修改密码,注意Mysql8密码设置规则必须是大小写字母+特殊符号+数字的类型）

```
 ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';
```

