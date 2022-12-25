POST请求

```
curl localhost:3000/api/json -X POST -d '{"hello": "world"}' --header "Content-Type: application/json"
```

Socks5临时代理

```bash
curl --noproxy "*" http://www.google.com
```

# 编译高版本的curl

依赖：openssl

```
wget https://www.openssl.org/source/openssl-1.1.1s.tar.gz --no-check-certificate
tar zxvf openssl-1.1.1s.tar.gz && cd openssl-1.1.1s
./config shared --prefix=/tmp/ssl
make -j `grep processor /proc/cpuinfo  | wc -l`
make install
```

依赖：c-ares（异步DNS解析）

```
wget https://github.com/c-ares/c-ares/releases/download/cares-1_18_1/c-ares-1.18.1.tar.gz
tar zxvf c-ares-1.18.1.tar.gz
./configure --prefix=/tmp/c-ares
make -j `grep processor /proc/cpuinfo  | wc -l`
make install
```

curl

```
wget https://curl.se/download/curl-7.86.0.tar.gz
./configure --prefix=/usr/local/curl-7.86 --without-nss --with-ssl=/tmp/ssl --enable-ares=/tmp/c-ares/
make -j `grep processor /proc/cpuinfo  | wc -l`
make install
```

