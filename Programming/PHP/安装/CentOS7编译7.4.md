下载源码

```
wget https://www.php.net/distributions/php-7.4.13.tar.gz
```

## 安装依赖

Yum

```shell
yum -y install wget vim pcre pcre-devel openssl openssl-devel libicu-devel gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel ncurses ncurses-devel curl curl-devel krb5-devel libidn libidn-devel openldap openldap-devel nss_ldap jemalloc-devel cmake boost-devel bison automake libevent libevent-devel gd gd-devel libtool* libmcrypt libmcrypt-devel mcrypt mhash libxslt libxslt-devel readline readline-devel gmp gmp-devel libcurl libcurl-devel openjpeg-devel sqlite-devel oniguruma-devel epel-release  libsqlite3x-devel  libsqlite-devlel php-ldap*
```

rpm包

```bash
wget https://rpms.remirepo.net/enterprise/7/remi/x86_64/oniguruma5php-devel-6.9.8-1.el7.remi.x86_64.rpm --no-check-certificate
wget https://rpms.remirepo.net/enterprise/7/remi/x86_64/oniguruma5php-6.9.8-1.el7.remi.x86_64.rpm --no-check-certificate
rpm -ivh oniguruma5php-6.9.8-1.el7.remi.x86_64.rpm  oniguruma5php-devel-6.9.8-1.el7.remi.x86_64.rpm 
```

gd相关依赖

> 如果下载缓慢可以用百度网盘
>
> 链接：https://pan.baidu.com/s/1i70vBhoR-f_0bwaS5dK30w 提取码: 2ggc 
>
> 路径：lib/Centos7_x86_64

```
echo "http://nchc.dl.sourceforge.net/project/mcrypt/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz
http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz
ftp://sunsite.unc.edu/pub/Linux/libs/graphics/t1lib-5.1.2.tar.gz
https://bitbucket.org/libgd/gd-libgd/downloads/libgd-2.1.0.tar.gz
https://src.fedoraproject.org/repo/pkgs/libvpx/libvpx-v1.3.0.tar.bz2/14783a148872f2d08629ff7c694eb31f/libvpx-v1.3.0.tar.bz2
https://src.fedoraproject.org/repo/pkgs/libtiff/tiff-4.0.3.tar.gz/051c1068e6a0627f461948c365290410/tiff-4.0.3.tar.gz
http://jaist.dl.sourceforge.net/project/libpng/libpng16/older-releases/1.6.12/libpng-1.6.12.tar.gz
https://master.dl.sourceforge.net/project/freetype/freetype2/2.5.3/freetype-2.5.3.tar.bz2
http://www.ijg.org/files/jpegsrc.v9a.tar.gz" | while read line ;do
wget -nc $line;done

```

- libmcrypt

  ```
  tar zxf libmcrypt-2.5.8.tar.gz && cd libmcrypt-2.5.8
  ./configure && make -j4 && make install && cd ..
  ```

- yasm

  ```
  tar zxf yasm-1.2.0.tar.gz && cd yasm-1.2.0
  ./configure && make -j4 && make install && cd ..
  ```

- libvpx

  ```
  tar xf libvpx-v1.3.0.tar.bz2 && cd libvpx-v1.3.0
  ./configure --prefix=/usr/local/libvpx --enable-shared --enable-vp9 && make -j4 && make install && cd ..
  ```

- tiff

  ```
  tar zxf tiff-4.0.3.tar.gz && cd tiff-4.0.3
  ./configure --prefix=/usr/local/tiff --enable-shared && make -j4 && make install && cd ..
  ```

- libpng

  ```
  tar xf  libpng-1.6.12.tar.gz && cd libpng-1.6.12
  ./configure --prefix=/usr/local/libpng --enable-shared && make -j4 && make install && cd ..
  ```

- freetype

  ```
  tar xf freetype-2.5.3.tar.bz2 && cd freetype-2.5.3
  ./configure --prefix=/usr/local/freetype --enable-shared && make -j4 && make install && cd ..
  ```

- jpeg

  ```
  tar xf jpegsrc.v9a.tar.gz && cd jpeg-9a
  ./configure --prefix=/usr/local/jpeg --enable-shared && make -j4 && make install && cd ..
  ```

- libgd

  ```
  tar xf libgd-2.1.0.tar.gz  && cd libgd-2.1.0
  ./configure --prefix=/usr/local/libgd --enable-shared --with-jpeg=/usr/local/jpeg --with-png=/usr/local/libpng --with-freetype=/usr/local/freetype --with-fontconfig=/usr/local/freetype --with-xpm=/usr/ --with-tiff=/usr/local/tiff --with-vpx=/usr/local/libvpx
  make -j4 && make install && cd ..
  ```

  



## 配置、编译与安装

```bash
export LD_LIBRARY_PATH=/usr/local/libgd/lib

./configure --prefix=/usr/local/services/php-7.4.13 -with-config-file-path=/usr/local/services/php-7.4.13/etc --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-mysqlnd-compression-support --with-iconv-dir --with-zlib --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --enable-intl --enable-gd --enable-ftp --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-soap -with-gettext --disable-fileinfo --enable-opcache --with-pear --enable-maintainer-zts --without-gdbm --with-jpeg --with-freetype
```

```
make -j `grep processor /proc/cpuinfo  | wc -l` && sudo make install
```

## 配置文件

配置文件：/usr/local/services/php-7.4.13/etc/php.ini

```ini
[PHP]
expose_php = Of
short_open_tag = On
cgi.fix_pathinfo=1
max_execution_time = 300
max_input_time = 300
memory_limit = 256M
post_max_size = 100M
upload_max_filesize = 10M
upload_max_filesize
[Date]
date.timezone = Asia/Shanghai
```

配置文件：/usr/local/services/php-7.4.13/etc/php-fpm.ini

```ini
listen=127.0.0.1:9000
include=/usr/local/services/php-7.4.13/etc/php-fpm.d/*.conf
```

配置文件：/usr/local/services/php-7.4.13/etc/www.conf

```ini
user = nginx 
group = nginx
listen = 127.0.0.1:9000
pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
```

启动命令参考

```
ExecStart=/usr/local/services/php-7.4.13/sbin/php-fpm -y /usr/local/services/php-7.4.13/etc/php-fpm.d/www.conf -c /usr/local/services/php-7.4.13/etc/php.ini
```



# 开机自启

systemd配置文件：/usr/lib/systemd/system/php.service

```
[Unit]
Description=php daemon

[Service]
Type=forking
User=root
Group=root
ExecStart=/usr/local/services/php-7.4.13/sbin/php-fpm -y /usr/local/services/php-7.4.13/etc/php-fpm.d/www.conf -c /usr/local/services/php-7.4.13/etc/php.ini
KillMode=process
KillSignal=SIGQUIT
Restart=on-failure
RestartSec=42s
```

