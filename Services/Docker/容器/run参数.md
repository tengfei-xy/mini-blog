# 网络

- bridge：桥接网络

  默认情况下启动的Docker容器，都是使用 bridge，Docker安装时创建的桥接网络，每次Docker容器重启时，会按照顺序获取对应的IP地址，这个就导致重启下，Docker的IP地址就变了

- none：无指定网络，不分配IP

  ```bash
  --network=none
  ```

- host： 主机网络

  此时，Docker 容器的网络会附属在主机上，两者是互通的。

  ```bash
  --network=host
  ```

  

- 创建自定义网络：(设置固定IP)

  1. 创建自定义网络，并且指定网段：172.18.0.0/16

     ```bash
     docker network create --subnet=172.18.0.0/16 mynetwork
     ```

  2. 启动容器并指定网络

     ```bash
     docker run -itd –name networkTest1 –net mynetwork –ip 172.18.0.2 centos:latest /bin/bash
     ```

     

