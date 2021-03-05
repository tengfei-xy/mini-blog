# zabbix 4.2.4安装
 安装环境 -> 下载编译 zabbix -> 配置 zabbix -> 完成

一、安装环境
1. 安装 Mysql
MySQL 安装方法
2. 安装 nginx
nginx 安装方法
3. 安装 PHP
PHP 安装方法
4. 安装依赖
$ yum install -→ net-snmp-devel

二、下载编译 zabbix
1. 下载和解包
$ wget https://nchc.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/4.2.4/zabbix-4.2.4.tar.gz
$ tar zxvf zabbix-4.2.4.tar.gz
$ cd zabbix-4.2.4

 2. 根据主机不同安装配置不同内容:
./configure --prefix/usr/lcoal/zabbix
服务器 配置选项:  --enable-server --with-net-snmp
代理 配置选项:  --enable-agent --with-mysql --enable-ipv6 --with-libcurl --with-libxml2

3. 安装
$ make install

三、配置 zabbix
1. 复制zabbix的php文件到 web 服务器下
cp -R frontends/php/nginx/html/zabbix/
注:以下采用 nginx 服务器并且 php运行在 127.0.0.1:9000
nginx.conf的配置如下：
location / {
root   html;
index  index.php;
}
location \~ \.php$ {
fastcgi\_param PATH\_INFO $fastcgi\_path\_info;
fastcgi\_param SCRIPT\_FILENAME $document\_root$fastcgi\_script\_name;
fastcgi\_split\_path\_info ^(.+\.php)(/.+)$;
include fastcgi\_params;
fastcgi\_pass 127.0.0.1:9000;
}
2. 修改zabbix.conf.php 的远程myql配置信息,如DBHost,DBPort,DPPassword 等信息

3. 创建 zabbix 数据库
$ gzip -d /usr/share/doc/zabbix-server-mysql-4.2.4/create.mysql.gz
复制/usr/share/doc/zabbix-server-mysql-4.2.4/create.gz 到 MySQL服务器所在的主机之下
mysql> create database zabbix character set utf8;
myslq> grant all on zabbix.\* to zabbix@'10.1.41.%' identified by 'MyPassword';
mysql> flush privileges;
mysql> quit;
$ mysql -u root -D zabbix  -p <create.sql 

四、完成
通过访问http://server\_ip//zabbix 来访问
启动 服务器: sbin/zabbix\_server -c etc/zabbix\_server.conf
启动 代理程序:sbin/zabbix\_agentd -c etc/zabbix\_agentd.conf
默认服务器日志文件：/tmp/zabbix\_server.log;
默认服务器启动端口:10050