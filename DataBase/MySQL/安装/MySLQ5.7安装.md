```bash
# MySQL 5.7安装
下载 带boost的mysql5.7
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-boost-5.7.27.tar.gz

# centos
yum -y install gcc gcc-c++ ncurses ncurses-devel bison libgcrypt perl make cmake bison-devel

# ubutnu 
apt-get install cmake libncurses5-dev  bison g++


cmake . -DWITH_BOOST=~/mysql-5.7.27/boost/boost_1_59_0 -DCMAKE_INSTALL_PREFIX=/usr/local/mysql-5.7.27 -DMYSQL_USER=mysql -DMYSQL_UNIX_ADDR=/usr/local/mysql-5.7.27/logs/mysql.sock -DMYSQL_TCP_PORT=3306  -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITH_FEDERATED_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 -DENABLED_LOCAL_INFILE=1 -DENABLE_DTRACE=0 -DDEFAULT_CHARSET=utf8mb4 -DDEFAULT_COLLATION=utf8mb4_general_ci -DWITH_EMBEDDED_SERVER=OFF

make -j `grep processor /proc/cpuinfo  | wc -l`
make install
cd /usr/local/mysql-5.7.27
groupadd mysql && useradd -r -g mysql -s /bin/false mysql
mkdir data logs etc 
touch /usr/local/mysql-5.7.27/logs/mysql-error.log
chown -R mysql:mysql ../mysql-5.7.27 && chmod -R 755 ../mysql-5.7.27
bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql-5.7.27 --datadir=/usr/local/mysql-5.7.27/data
```

