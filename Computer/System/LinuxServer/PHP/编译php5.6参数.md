# 编译PHP5.6参数
nginx 下的 php安装

```bash
$ sudo yum install -→ epel-release bzip2 bzip2-devel curl curl-devel libmcrypt-devel readline-devel libxml2 libxml2-devel libjpeg-devel libpng-devel libpng freetype-devel libevent-devel
$ wget http://cn2.php.net/distributions/php-5.6.30.tar.gz
$ tar zxvf php-5.6.30.tar.gz
$ cd php-5.6.30
$ ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-inline-optimization --disable-debug --disable-rpath --enable-shared --enable-opcache --enable-fpm --with-fpm-user=www --with-png-dir --with-freetype-dir --with-jpeg-dir --with-gd --with-fpm-group=www --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-gettext --enable-mbstring --with-iconv --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir=/usr/ --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-zlib --enable-zip --with-bz2 --with-readline

$ make
$ make install
```



注:
如果是 apache ,需要`--with-apxs2=/usr/local/apache/bin/apxs`
nginx则不需要`--with-mcrypt`