文档作者微信：Melta_（可联系获取安装包资源）

# GBASE 8a MPP Cluster集群部署

部署参考文档：https://juejin.cn/post/7014696094894391327

## 部署说明

硬件配置要求

| 硬件           | 配置                                                         |
| -------------- | ------------------------------------------------------------ |
| CPU            | 最低配置:1×2核 2.0GHz<br />推荐配置:高于2×4核 2.0GHz         |
| 内存           | 最低配置:32GB<br />推荐配置:128GB                            |
| 网卡           | 业务平面使用 1GE<br />数据平面使用 10GE                      |
| 光驱           | CD-ROM                                                       |
| 硬盘           | 最低配置:SATA 7200 rpm 100GB<br />推荐配置:SAS 10k rpm 1TB   |
| 磁盘 RAID 配置 | 管理节点:操作系统所在盘独占一个RAID组，且RAID组级别 为 RAID1;非操作系统所在盘独占一个 RAID 组，且 RAID 组级别为RAID5。<br />数据节点:操作系统所在盘独占一个RAID组，且RAID组级别 为 RAID1;非操作系统所在盘配置 RAID5。 |
| 磁盘空间       | 管理节点:操作系统盘≥600GB，每个数据库所在磁盘≥600GB。<br />数据节点:操作系统盘≥600GB，每个非操作系统盘≥600GB。 |

支持操作系统

| 操作系统名称  | 版本  | 结构   |
| ------------- | ----- | ------ |
| Red Hat Linux | 7.x   | X86_64 |
| CentOS        | 7.x   | X86_64 |
| SUSE          | 11-12 | X86_64 |
| 中标麒麟      | 6.0   | X86_64 |

两种部署方式

一、coordinator 节点与 data 节点分别部署在不同服务器上:

1. 管理集群的节点单独部署在管理服务器上。
2. 数据集群的节点独立部署在数据服务器上，data 节点可跨网段部署，只要保证网络通信质量即可。
3. 根据集群规模和业务并发量，Coordinator 节点个数建议 3~11 的奇数个。

   单个 coordinator 节点建议的业务并发量不超过 300

   小规模集群(data 节点数<=10)设置 3 个 coordinator 节点

   中等规模集 群(data 节点数在[10,30]之间)设置 5个 coordinator 节点

   大规模集群(data 节点数在[30,100]之间)设置 7 或 9 个 coordinator 节点

   超大规模集群(data 节点数>=100)设置 9 或 11 个 coordinator 节点

二、coordinator 节点和 data 节点共同部署在同一个服务器上:

1. 管理集群节点和数据集群的节点部署在同一个服务器上

2. 小规模集群可采用方案 2 的方式部署，中等规模及以上的集群建议 coordinator 节点和 data 节点分开部署

   

端口说明

| 所属段      | 端口名称           | 含义                     | 默认值 | 修改方式                                                     |
| ----------- | ------------------ | ------------------------ | ------ | ------------------------------------------------------------ |
| gclusterd   | port               | 集群对外提供服务端口     | 5258   | 需要同对外提供服务端口 值一致, 要求所有节点一致              |
| gbased      | port               | 单机对外连接端口         | 5050   | gnode 对外接口，需要和 gcluster_gnode_port 一致, 要求所有节点一致 |
| 无          | recover_monit_port | 监控收集信息端口         | 6268   | 要求所有节点一致                                             |
| 无          | SERVER_PORT        | 同步连接端口             | 5288   | 要求所有节点一致                                             |
| config/port | port               | 日志收集工具对外服务端口 | 6957   | 要求所有节点一致                                             |
| config/port | port               | 日志收集工具对外服务端口 | 6957   | 要求所有节点一致                                             |
| gcware      | gcluster_port      | 检测 gclusterd状态端口   | 5288   | 要求和 gcluster 对外服务端口一致                             |
| gcware      | gnode_port         | 检测 gbased状态端口      | 5050   | 要求和 gnode 对外服务端口一致                                |
| gcware      | syncserver_port    | 检测 syncserver状态端口  | 5288   | 要求和 synctool 配置端口一致                                 |
| gcware      | node_ssh_port      | 检测系统是否在线端口     | 22     | 需要和 ssh 服务端口配置一致                                  |
| gcware      | gcware_client      | gcware节点通讯           | 5918   | 要求所有 gcware 一致                                         |
| gcware      | gcware_server      | 外部连接 gcware节点      | 5919   | 要求所有 gcware 一致                                         |

## 初始化

RHEL7安装依赖

```
yum install -y libstdc++ cyrus-sasl-lib zlib nspr libidn libuuid ncurses-libs nss-util openldap libgcc nss glibc nss-softokn-freebl killall net-tools ntp bzip2
```

关闭防火墙

```
systemctl stop firewalld
systemctl disable firewalld
```

关闭SELINUX

```
setenforce 0
sed -i 's/=enforcing/=disabled/g'  /etc/selinux/config
```

启动ntp

```
systemctl start ntpd
systemctl enable ntpd
```

ntp服务器执行

```bash
cat > /etc/ntp.conf <<EOF
driftfile /var/lib/ntp/drift
server 127.127.1.0
includefile /etc/ntp/crypto/pw
keys /etc/ntp/keys
disable monitor
EOF
```

ntp客户端其他节点执行

```bash
cat > /etc/ntp.conf <<EOF
driftfile /var/lib/ntp/drift
restrict 127.0.0.1 
restrict ::1
server 192.168.3.71
Fudge 192.168.3.71 stratum 10
includefile /etc/ntp/crypto/pw
keys /etc/ntp/keys
disable monitor
EOF
```

修改内核参数

```
cat > /etc/sysctl.conf << EOF
kernel.core_uses_pid = 1
net.core.netdev_max_backlog = 262144
net.core.rmem_default = 8388608
net.core.rmem_max = 16777216
net.core.somaxconn = 32767
net.core.wmem_default = 8388608
net.core.wmem_max = 16777216
net.ipv4.tcp_max_syn_backlog = 262144
net.ipv4.tcp_rmem = 4096 87380 4194304
net.ipv4.tcp_sack = 1
net.ipv4.ip_local_reserved_ports = 5050,5258,5288,6666,6268
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 16384 4194304
vm.vfs_cache_pressure = 1024
vm.swappiness = 1
vm.overcommit_memory = 0
vm.zone_reclaim_mode = 0
vm.max_map_count = 117604
#vm.min_free_kbytes = 188166 #Commented out by gcluster
vm.min_free_kbytes = 188166
EOF
sysctl -p
```

设置虚拟内存

```
cat >> /etc/security/limits.conf <<EOF
* soft as unlimited
* hard as unlimited
EOF
```

修改/etc/default/grub，找到GRUB_CMDLINE_LINUX 这一行，在双引号内加入

```
elevator=deadline
transparent_hugepage=never
```

修改hosts

```
echo "127.0.0.1 $(hostname)" >> /etc/hosts
```

设定system.conf

```
sed -i 's#^\#DefaultTasksMax=.*#DefaultTasksMax=infinity#g' /etc/systemd/system.conf
systemctl daemon-reexec
```

其他操作

1. Swap 分区大小设置为和物理内存大小相同
2. 关闭CPU超频

创建用户

```
groupadd gbase
useradd -p $(openssl passwd -1 "gbase") -g gbase -d /home/gbase -m gbase
```

创建安装目录

```
mkdir /opt
chown -R gbase.gbase /opt
```



# 安装集群

解压安装包

```
tar xf GBase8a_MPP_Cluster-License-9.5.3.14-redhat7.3-x86_64.tar.bz2
```

利用SetSysEnv.py自动设置系统参数，该文件不属于集群，需要用户保证，需要在每个节点执行

```bash
su - root -c "/home/gbase/gcinstall/SetSysEnv.py --dbaUser=gbase --installPrefix=/opt"
```

查看SetSysEnv.py执行日志

```bash
less /tmp/SetSysEnv.log
```

编辑gcinstall/demo.options

```ini
installPrefix= /opt
coordinateHost = 192.168.3.72
coordinateHostNodeID =72 
dataHost = 192.168.3.72 
#existCoordinateHost =
#existDataHost =
#existGcwareHost=
gcwareHost = 192.168.3.72
gcwareHostNodeID = 72
dbaUser = gbase
dbaGroup = gbase
dbaPwd = 'gbase'
rootPwd = 'bb123456'
#rootPwdFile = rootPwd.json
#characterSet = utf8
#dbPort = 5258
#sshPort = 22
```

使用gbase用户执行安装

```
gcinstall/gcinstall.py --silent=demo.options
```

安装部分日志

```
192.168.3.72         	install gcware and cluster on host 192.168.3.72 successfully.
Starting all gcluster nodes ...
 start cluster service failed on host 192.168.3.72.
adding new datanodes to gcware ...
```

安装后的目录：

| 目录名            | 功能                                                         |
| ----------------- | ------------------------------------------------------------ |
| Gcware            | 该目录在安装目录下，存储 gcware 模组的相关文件，包括配置 文件和日志等，coordinator 节点有该目录，gnode 节点没有该目 录。 |
| gcluster          | 该目录在安装目录下，存储 gcluster 模组的相关文件，包括配置 文件、日志和元数据等。 |
| gnode             | 该目录在安装目录下，存储 gnode 模组的相关文件，包括配置文 件、日志和用户数据等。 |
| /home/dbaUser/ .g | .gbase_profile 文件是数据库安装用户下的环境变量文件。        |

安装成功后，使用gbase用户生效环境变量

```
. ~/.gbase_profile
```

使用gbase用户通过 gcadmin 查看集群状况，注：集群状态和节点状态都是 `CLOSE`，原因是因为因为没有注册 `License` 授权，属于正常现象

```
[gbase@gbase gcinstall]$ gcadmin
CLUSTER STATE:         ACTIVE

====================================
| GBASE GCWARE CLUSTER INFORMATION |
====================================
| NodeName |  IpAddress   | gcware |
------------------------------------
| gcware1  | 192.168.3.72 |  OPEN  |
------------------------------------
======================================================
|       GBASE COORDINATOR CLUSTER INFORMATION        |
======================================================
|   NodeName   |  IpAddress   | gcluster | DataState |
------------------------------------------------------
| coordinator1 | 192.168.3.72 |  CLOSE   |     0     |
------------------------------------------------------
=============================================================
|         GBASE CLUSTER FREE DATA NODE INFORMATION          |
=============================================================
| NodeName  |  IpAddress   | gnode | syncserver | DataState |
-------------------------------------------------------------
| FreeNode1 | 192.168.3.72 | CLOSE |    OPEN    |     0     |
-------------------------------------------------------------
0 virtual cluster
1 coordinator node
1 free data node
```

进行LiCENSE认证，使用gabse用户生成到/home/gbase/finger.txt，

```
./gethostsid -n 192.168.3.72 -f /home/gbase/finger.txt -u gbase -p gbase
```

将finger.txt作为附件发送给partner@gbase.cn，注：该邮箱更新于：2021年12月16日，本人在这时间进行申请。邮箱正文参考

```
客户名称: xxx
项目名称: 2021年X月认证培训
申请人: xxx
申请原因: 培训学习
有效期: 3个月
操作系统名称及版本: Red Hat Enterprise Linux Server release 7.3 (Maipo)
8a集群版本: GBase8a_MPP_Cluster-License-9.5.2.39-redhat7.3-x86_64.tar.bz2
```

导入License

```
[gbase@gbase gcinstall]$ ./License -n 192.168.3.72 -u gbase -p gbase -f  ~/mpp.lic
=============================================================
Successful node nums:	1
==============================================================
```

查看License状况

```
./gcinstall/chkLicense -n 192.168.3.72 -u gbase -p gbase
======================================================================
192.168.3.72
is_exist:yes
version:trial
expire_time:20220315
is_valid:yes
```

重启集群

```
[gbase@gbase ~]$  gcluster_services all restart
Stopping GCMonit success!
Stopping gcrecover :                                       [  OK  ]
Stopping gcluster :                                        [  OK  ]
Stopping gbase :                                           [  OK  ]
Stopping syncserver :                                      [  OK  ]
Starting gcluster :                                        [  OK  ]
Starting gcrecover :                                       [  OK  ]
Starting gbase :                                           [  OK  ]
Starting syncserver :                                      [  OK  ]
Starting GCMonit success!
```

再次查看状态

```
[gbase@gbase ~]$ gcadmin
CLUSTER STATE:         ACTIVE

====================================
| GBASE GCWARE CLUSTER INFORMATION |
====================================
| NodeName |  IpAddress   | gcware |
------------------------------------
| gcware1  | 192.168.3.72 |  OPEN  |
------------------------------------
======================================================
|       GBASE COORDINATOR CLUSTER INFORMATION        |
======================================================
|   NodeName   |  IpAddress   | gcluster | DataState |
------------------------------------------------------
| coordinator1 | 192.168.3.72 |   OPEN   |     0     |
------------------------------------------------------
=============================================================
|         GBASE CLUSTER FREE DATA NODE INFORMATION          |
=============================================================
| NodeName  |  IpAddress   | gnode | syncserver | DataState |
-------------------------------------------------------------
| FreeNode1 | 192.168.3.72 | OPEN  |    OPEN    |     0     |
-------------------------------------------------------------

0 virtual cluster
1 coordinator node
1 free data node
```

生成分布信息

```
[gbase@gbase gcinstall]$ gcadmin distribution gcChangeInfo.xml p 1 d 0
gcadmin generate distribution ...

[warning]: parameter [d num] is 0, the new distribution will has no segment backup
please ensure this is ok, input [Y,y] or [N,n]: y
NOTE: node [192.168.3.72] is coordinator node, it shall be data node too
gcadmin generate distribution successful

```

再次查看,DistributionId变为1

```
[gbase@gbase gcinstall]$ gcadmin
CLUSTER STATE:         ACTIVE
VIRTUAL CLUSTER MODE:  NORMAL

====================================
| GBASE GCWARE CLUSTER INFORMATION |
====================================
| NodeName |  IpAddress   | gcware |
------------------------------------
| gcware1  | 192.168.3.72 |  OPEN  |
------------------------------------
======================================================
|       GBASE COORDINATOR CLUSTER INFORMATION        |
======================================================
|   NodeName   |  IpAddress   | gcluster | DataState |
------------------------------------------------------
| coordinator1 | 192.168.3.72 |   OPEN   |     0     |
------------------------------------------------------
=========================================================================================================
|                                    GBASE DATA CLUSTER INFORMATION                                     |
=========================================================================================================
| NodeName |                IpAddress                 | DistributionId | gnode | syncserver | DataState |
---------------------------------------------------------------------------------------------------------
|  node1   |               192.168.3.72               |       1        | OPEN  |    OPEN    |     0     |
---------------------------------------------------------------------------------------------------------
```

## 初始化

使用gbase用户执行initnodedatamap，密码为空

```bash
[gbase@gbase gcinstall]$ gccli -u root -p
Enter password: 

GBase client 9.5.3.14.121230. Copyright (c) 2004-2021, GBase.  All Rights Reserved.
```

```bash
gbase> initnodedatamap;
Query OK, 0 rows affected (Elapsed: 00:00:03.09)
```

