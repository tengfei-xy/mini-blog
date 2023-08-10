# 19c-docker

## 目录

-   [Centos7 Docker 安装 19.3 RAC](#Centos7-Docker-安装-193-RAC)
-   [环境准备](#环境准备)
-   [ConnectionManager](#ConnectionManager)
-   [stronge](#stronge)
-   [DNS](#DNS)
-   [RAC Database](#RAC-Database)

# Centos7 Docker 安装 19.3 RAC

1.  全程联网（需要连接yum.oralce.com）
2.  使用DNS、存储服务器均从容器构建
3.  单主机搭建rac节点
4.  [Github](https://github.com/oracle/docker-images/tree/main/OracleDatabase/RAC/OracleRealApplicationClusters "Github")

| 名称        | IP            |
| --------- | ------------- |
| nfs       | 192.168.17.25 |
| dns       | 172.16.1.25   |
| rac node1 | 17.16.1.150   |
| rac node2 | 17.16.1.151   |

# 环境准备

每个节点8G的内存，硬盘空间：至少50G（否则安装失败），数据默认保存在/oradata。不支持挂载点/oradata

安装docker（20版本以上）

```纯文本
curl -fsSL https://get.docker.com/ | sh
```

在/usr/lib/systemd/system/docker.service文件的ExecStart=行尾添加，并启动docker

```纯文本
--selinux-enabled --cpu-rt-runtime=950000
```

构建docker网络

```纯文本
docker network create --driver=bridge --subnet=172.16.1.0/24 rac_pub1_nw
docker network create --driver=bridge --subnet=192.168.17.0/24 rac_priv1_nw
```

设置内核参数

```纯文本
fs.file-max = 6815744
net.core.rmem_max = 4194304
net.core.rmem_default = 262144
net.core.wmem_max = 1048576
net.core.wmem_default = 262144
net.core.rmem_default = 262144
```

# ConnectionManager

复制安装包(LINUX.X64\_193000\_client.zip)到OracleConnectionManager/19.3.0

构建镜像

```纯文本
cd OracleConnectionManager/dockerfiles
./buildDockerImage.sh -v 19.3.0
```

构建容器

```纯文本
/usr/bin/docker run -d --hostname racnode-cman1 --dns-search=example.com \
--network=rac_pub1_nw --ip=172.16.1.15 \
-e DOMAIN=example.com -e PUBLIC_IP=172.16.1.15 \
-e PUBLIC_HOSTNAME=racnode-cman1 -e SCAN_NAME=racnode-scan \
-e SCAN_IP=172.16.1.70 --privileged=false \
-p 1521:1521 --name racnode-cman oracle/client-cman:19.3.0
```

查看日志

```纯文本
docker logs -f racnode-cman
```

完成racnode-cman容器设置时，您应该会看到以下内容：

```纯文本
11-05-2021 20:29:03 UTC :  : ################################################
11-05-2021 20:29:03 UTC :  :  CONNECTION MANAGER IS READY TO USE!            
11-05-2021 20:29:03 UTC :  : ################################################
11-05-2021 20:29:03 UTC :  : cman started sucessfully
```

# stronge

构建存储服务器镜像

```纯文本
cd OracleRACStorageServer/dockerfiles
./buildDockerImage.sh -v 19.3.0
```

关闭selinux

```纯文本
sed -i 's/=enforcing/=disabled/g'  /etc/selinux/config && setenforce 0
```

宿主系统安装nfs

```纯文本
yum -y install nfs-utils
```

构建存储服务器容器

```纯文本
docker run -d -t --hostname racnode-storage \
--dns-search=example.com  --cap-add SYS_ADMIN \
--volume /docker_volumes/asm_vol/$ORACLE_SID:/oradata --init \
--network=rac_priv1_nw --ip=192.168.17.25 --tmpfs=/run  \
--volume /sys/fs/cgroup:/sys/fs/cgroup:ro \
--name racnode-storage oracle/rac-storage-server:19.3.0
```

**重要信息：**

1.  在容器启动期间，将在/oradata下创建名为asm\_disk0\[1-5].img的5个文件。如果文件已经存在，则不会重新创建。这些文件可用于RAC容器中的ASM存储。
2.  将目录暴露给至少有60GB的容器。在上述示例中，我们使用/docker\_volumes/asm\_vol/\$ORACLE\_SID，您需要根据您的环境更改值。在容器内，它将是/oradata，不要更改此。

查看日志

```纯文本
docker logs -f racnode-storage
```

您应该在docker日志输出中看到以下内容：

```纯文本
####################################################
 NFS Server is up and running                       
 Create NFS volume for /oradata/         
####################################################
0
```

创建NFS卷

```纯文本
docker volume create --driver local \
--opt type=nfs \
--opt   o=addr=192.168.17.25,rw,bg,hard,tcp,vers=3,timeo=600,rsize=32768,wsize=32768,actimeo=0 \
--opt device=:/oradata racstorage
```

# DNS

构建镜像

```纯文本
cd OracleDNSServer/dockerfiles
./buildContainerImage.sh -v latest
```

构建DNS服务器容器

```纯文本
docker run -d  --name racdns \
 --hostname rac-dns  \
 --dns-search="example.com" \
 --cap-add=SYS_ADMIN  \
 --network  rac_pub1_nw \
 --ip 172.16.1.25 \
 --sysctl net.ipv6.conf.all.disable_ipv6=1 \
 --env SETUP_DNS_CONFIG_FILES="setup_true" \
 --env DOMAIN_NAME="example.com" \
 --env RAC_NODE_NAME_PREFIX="racnode" \
 oracle/rac-dnsserver:latest
```

查看日志

```纯文本
docker logs -f racdns
```

完成时应输出

```纯文本
11-05-2021 20:57:12 UTC :  : ################################################
11-05-2021 20:57:12 UTC :  :  DNS Server IS READY TO USE!            
11-05-2021 20:57:12 UTC :  : ################################################
11-05-2021 20:57:12 UTC :  : DNS Server Started Successfully
```

# RAC Database

复制安装包

```纯文本
安装包:
LINUX.X64_193000_grid_home.zip LINUX.X64_193000_db_home.zip 
目的路径:
OracleRealApplicationClusters/dockerfiles/19.3
```

构建镜像

```纯文本
cd OracleRealApplicationClusters/dockerfiles/
./buildContainerImage.sh -v 19.3.0
# /opt/scripts/install/installGridBinaries.sh: line 57:  : command not found
```

参考：[github文档](https://github.com/oracle/docker-images/tree/main/OracleDatabase/RAC/OracleRealApplicationClusters "github文档")

仅创建不修改容器的hosts文件

```纯文本
mkdir /opt/containers
touch /opt/containers/rac_host_file
```

设定常用密码

```纯文本
mkdir /opt/.secrets/
openssl rand -hex 64 -out /opt/.secrets/pwd.key
echo "oracle" >/opt/.secrets/common_os_pwdfile
```

加密文件

```纯文本
openssl enc -aes-256-cbc -salt -in /opt/.secrets/common_os_pwdfile -out /opt/.secrets/common_os_pwdfile.enc -pass file:/opt/.secrets/pwd.key
rm -f /opt/.secrets/common_os_pwdfile
chmod 400 /opt/.secrets/common_os_pwdfile.enc
chmod 400 /opt/.secrets/pwd.key
```

如果您使用Oracle Connection Manager从主机外部访问Oracle RAC数据库，您需要在容器创建命令中添加以下变量。

```纯文本
-e CMAN_HOSTNAME=(CMAN_HOSTNAME) -e CMAN_IP=(CMAN_IP)
```

使用oracle rac strong容器构建racnode1

```纯文本
docker create -t -i \
  --hostname racnode1 \
  --volume /boot:/boot:ro \
  --volume /dev/shm \
  --tmpfs /dev/shm:rw,exec,size=4G \
  --volume /opt/containers/rac_host_file:/etc/hosts  \
  --volume /opt/.secrets:/run/secrets:ro \
  --dns=172.16.1.25 \
  --dns-search=example.com \
  --privileged=false \
  --volume racstorage:/oradata \
  --cap-add=SYS_NICE \
  --cap-add=SYS_RESOURCE \
  --cap-add=NET_ADMIN \
  -e DNS_SERVERS="172.16.1.25" \
  -e NODE_VIP=172.16.1.160  \
  -e VIP_HOSTNAME=racnode1-vip  \
  -e PRIV_IP=192.168.17.150  \
  -e PRIV_HOSTNAME=racnode1-priv \
  -e PUBLIC_IP=172.16.1.150 \
  -e PUBLIC_HOSTNAME=racnode1  \
  -e SCAN_NAME=racnode-scan \
  -e OP_TYPE=INSTALL \
  -e DOMAIN=example.com \
  -e ASM_DISCOVERY_DIR=/oradata \
  -e ASM_DEVICE_LIST=/oradata/asm_disk01.img,/oradata/asm_disk02.img,/oradata/asm_disk03.img,/oradata/asm_disk04.img,/oradata/asm_disk05.img  \
  -e CMAN_HOSTNAME=racnode-cman1 \
  -e CMAN_IP=172.16.1.15 \
  -e COMMON_OS_PWD_FILE=common_os_pwdfile.enc \
  -e PWD_KEY=pwd.key \
  --restart=always \
  --tmpfs=/run -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
  --cpu-rt-runtime=95000 \
  --ulimit rtprio=99  \
  --name racnode1 \
  oracle/database-rac:19.3.0

```

<https://github.com/oracle/docker-images/issues/1658>

将网络分配给RAC容器

```纯文本
docker network disconnect bridge racnode1
docker network connect rac_pub1_nw --ip 172.16.1.150 racnode1
docker network connect rac_priv1_nw --ip 192.168.17.150  racnode1
```

启动第一个容器(听说需要40分钟)

```纯文本
docker start racnode1
```

查看日志

```纯文本
docker logs -f racnode1
```

您应该会在末尾看到数据库创建成功消息：

```纯文本
11-05-2021 23:05:36 UTC :  : Generating DB Responsefile Running DB creation
11-05-2021 23:05:36 UTC :  : Running DB creation

11-05-2021 23:23:00 UTC :  : Checking DB status
11-05-2021 23:23:03 UTC :  : #################################################################
11-05-2021 23:23:03 UTC :  :  Oracle Database ORCLCDB is up and running on racnode1    
11-05-2021 23:23:03 UTC :  : #################################################################
11-05-2021 23:23:03 UTC :  : Running User Script oracle user
11-05-2021 23:23:03 UTC :  : Setting Remote Listener
11-05-2021 23:23:03 UTC :  : 172.16.1.15
11-05-2021 23:23:03 UTC :  : Executing script to set the remote listener
11-05-2021 23:23:05 UTC :  : ####################################
11-05-2021 23:23:05 UTC :  : ORACLE RAC DATABASE IS READY TO USE!
11-05-2021 23:23:05 UTC :  : ####################################
```

如安装失败，可登录容器，查看`/tmp/orod.log`或`$GRID_BASE/diag/crs`

```纯文本
docker exec -i -t racnode1 /bin/bash
```

oracle用户变量

```bash
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=/u01/app/oracle/product/19.3.0/dbhome_1
export OPATCH=$ORACLE_HOME/OPatch
export ORACLE_SID=ORCLCDB1
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export PATH=$PATH:$ORACLE_HOME/bin:$OPATCH
```

创建第二个节点

```纯文本
docker create -t -i \
  --hostname racnode2 \
  --volume /dev/shm \
  --tmpfs /dev/shm:rw,exec,size=4G  \
  --volume /boot:/boot:ro \
  --dns-search=example.com  \
  --volume /opt/containers/rac_host_file:/etc/hosts \
  --volume /opt/.secrets:/run/secrets:ro \
  --dns=172.16.1.25 \
  --dns-search=example.com \
  --privileged=false \
  --volume racstorage:/oradata \
  --cap-add=SYS_NICE \
  --cap-add=SYS_RESOURCE \
  --cap-add=NET_ADMIN \
  -e DNS_SERVERS="172.16.1.25" \
  -e EXISTING_CLS_NODES=racnode1 \
  -e NODE_VIP=172.16.1.161  \
  -e VIP_HOSTNAME=racnode2-vip  \
  -e PRIV_IP=192.168.17.151  \
  -e PRIV_HOSTNAME=racnode2-priv \
  -e PUBLIC_IP=172.16.1.151  \
  -e PUBLIC_HOSTNAME=racnode2  \
  -e DOMAIN=example.com \
  -e SCAN_NAME=racnode-scan \
  -e ASM_DISCOVERY_DIR=/oradata \
  -e ASM_DEVICE_LIST=/oradata/asm_disk01.img,/oradata/asm_disk02.img,/oradata/asm_disk03.imgv,/oradata/asm_disk04.img,/oradata/asm_disk05.img \
  -e ORACLE_SID=ORCLCDB \
  -e OP_TYPE=ADDNODE \
  -e COMMON_OS_PWD_FILE=common_os_pwdfile.enc \
  -e PWD_KEY=pwd.key \
  --tmpfs=/run -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
  --cpu-rt-runtime=95000 \
  --ulimit rtprio=99  \
  --restart=always \
  --name racnode2 \
  oracle/database-rac:19.3.0
```

将网络分配给RAC容器

```纯文本
docker network disconnect bridge racnode2
docker network connect rac_pub1_nw --ip 172.16.1.151 racnode2
docker network connect rac_priv1_nw --ip 192.168.17.151  racnode2
```

启动第一个容器(听说需要40分钟)

```纯文本
docker start racnode2
```

查看日志

```纯文本
docker logs -f racnode2
```

您应该会在末尾看到数据库创建成功消息：

```纯文本
####################################
ORACLE RAC DATABASE IS READY TO USE!
####################################
```

如安装失败，可登录容器，查看`/tmp/orod.log`或`$GRID_BASE/diag/crs`

```纯文本
docker exec -i -t racnode2 /bin/bash
```
