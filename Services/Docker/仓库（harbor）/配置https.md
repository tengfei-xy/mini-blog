> 参考配置：
>
> https://www.cnblogs.com/linuxws/p/12810887.html
>
> https://www.cnblogs.com/cjwnb/p/13441071.html

# 一、生成证书

生成CA证书私钥

```
openssl genrsa -out ca.key 4096
```

下方的环境变量换成你的harbor服务器名或IP

```
domain=harbor.dev.dbsinan.com
```

生成CA证书

```bash
openssl req -x509 -new -nodes -sha512 -days 3650 \
 -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=${domain}" \
 -key ca.key \
 -out ca.crt
```

生成私钥

```bash
openssl genrsa -out ${domain}.key 4096
```

生成证书签名请求（CSR）

```bash
openssl req -sha512 -new \
    -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=${domain}" \
    -key ${domain}.key \
    -out ${domain}.csr
```

 生成一个x509 v3扩展文件

- 域名访问

  ```bash
  cat > v3.ext <<-EOF
  authorityKeyIdentifier=keyid,issuer
  basicConstraints=CA:FALSE
  keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
  extendedKeyUsage = serverAuth
  subjectAltName = @alt_names
  
  [alt_names]
  DNS.1=${domain}
  DNS.2=${domain}
  DNS.3=${domain}
  EOF
  ```

  

- IP访问

  ```bash
  cat > v3.ext <<-EOF
  authorityKeyIdentifier=keyid,issuer
  basicConstraints=CA:FALSE
  keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
  extendedKeyUsage = serverAuth
  subjectAltName = IP:192.168.31.200
  EOF
  ```

  

使用该v3.ext文件为您的Harbor主机生成证书

```bash
openssl x509 -req -sha512 -days 3650 \
    -extfile v3.ext \
    -CA ca.crt -CAkey ca.key -CAcreateserial \
    -in ${domain}.csr \
    -out ${domain}.crt
```

生成客户端证书

```bash
openssl x509 -inform PEM -in ${domain}.crt -out ${domain}.cert
```

复制到harbor的data_volume路径，默认是/data

```
# 默认不存在，手动创建
mkdir /data/cert
cp ca.crt ${domain}.crt ${domain}.key /data/cert
```

将服务器证书，密钥和CA文件复制到Harbor主机上的Docker证书文件夹中

```
mkdir -p /etc/docker/certs.d/${domain}
cp ca.crt ${domain}.cert ${domain}.key /etc/docker/certs.d/${domain}
```



# 二、更新harbor

更新harbor.yaml，配置https部分

```
# https related config
https:
#   https port for harbor, default is 443
  port: 443
#   The path of cert and key files for nginx
  certificate: /data/cert/harbor.dev.dbsinan.com.crt
  private_key: /data/cert/harbor.dev.dbsinan.com.key
```

运行./prepare脚本将nginx配置为使用HTTPS

```
./prepare
```

重启

```bash
docker-compose down -v
docker-compose up -d
```

此时可以用docker login 登录，或者https://域名访问harbor后台管理



问题：如果docker login提示 certificate signed by unknown authority。

解决：将hardor服务器的/etc/docker/certs.d/下的证书复制到每个docker login的主机下/etc/docker/certs.d/。参考文档：https://docs.docker.com/registry/insecure/



