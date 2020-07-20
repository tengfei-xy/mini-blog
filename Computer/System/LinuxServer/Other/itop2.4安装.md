# itop2.4安装
安装环境 -> 下载解包 iTop -> 前端配置

一、安装环境
1. 安装 Mysql
MySQL 安装方法
2. 安装 nginx
nginx 安装方法
3. 安装 PHP
PHP 安装方法
4. 安装依赖
$ sudo yum install -→ unzip

二、 下载解包 iTop
$ wget https://nchc.dl.sourceforge.net/project/itop/itop/2.6.1/iTop-2.6.1-4463.zip
$ unzip iTop-2.6.1-4463
$ mv web/ nginx/html/itop

三、前端配置
进入http://server\_ip/itop/setup/来初始化 iTop
完成后进入 http://server\_ip/itop/index.php 来进入