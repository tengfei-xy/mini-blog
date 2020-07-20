```bash
# MySQL 5.7安装
下载 带boost的mysql5.7
$ wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.27.tar.gz

yum -→ install gcc gcc-c++ ncurses ncurses-devel bison libgcrypt perl make cmake bison-devel
编译数据库
$ cmake . -DWITH_BOOST=/home/tengfei/mysql-5.7.27/boost/boost_1_59_0 -DCMAKE_INSTALL_PREFIX=/usr/local/mysql-5.7.27 -DMYSQL_USER=mysql -DMYSQL_UNIX_ADDR=/usr/local/mysql-5.7.27/logs/mysql.sock -DMYSQL_TCP_PORT=3306  -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_FEDERATED_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 -DENABLED_LOCAL_INFILE=1 -DENABLE_DTRACE=0 -DDEFAULT_CHARSET=utf8mb4 -DDEFAULT_COLLATION=utf8mb4_general_ci -DWITH_EMBEDDED_SERVER=OFF

$ make
$ make install
$ cd /usr/local/mysql
$ groupadd mysql && useradd -r -g mysql -s /bin/false mysql
$ chown -R mysql:mysql ./* && chmod -R 750 ./*
$ bin/mysql_install_db --user=mysql --basedir=/usr/local/mysql5.7 --datadir=/usr/local/mysql5.7/data
ln -s /var/lib/mysql/mysql.sock /tmp/mysql.sock


cmake -DCMAKE\_INSTALL\_PREFIX=/usr/local/mysql-5.7.25 \
-DMYSQL\_DATADIR=/usr/local/mysql-5.7.25/data \
-DSYSCONFDIR=/usr/local/mysql-5.7.25/etc \
-DMYSQL\_UNIX\_ADDR=/usr/local/mysql-5.7.25/logs/mysql.sock \
-DMYSQL\_TCP\_PORT=3306 \
-DWITH\_BOOST=/root/mysql-5.7.25/boost\_1\_59\_0 \
-DWITH\_INNOBASE\_STORAGE\_ENGINE=1 \
-DWITH\_PARTITION\_STORAGE\_ENGINE=1 \
-DWITH\_FEDERATED\_STORAGE\_ENGINE=1 \
-DWITH\_BLACKHOLE\_STORAGE\_ENGINE=1 \
-DWITH\_MYISAM\_STORAGE\_ENGINE=1 \
-DENABLED\_LOCAL\_INFILE=1 \
-DENABLE\_DTRACE=0 \
-DDEFAULT\_CHARSET=utf8mb4 \
-DDEFAULT\_COLLATION=utf8mb4\_general\_ci \
-DWITH\_EMBEDDED\_SERVER=OFF \
```

