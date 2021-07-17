# Centos7.8静默安装11gRAC

rpm、安装包保存于[百度网盘](https://pan.baidu.com/s/1KEb2QFoM4rlNHJOq1ljSTQ)，提取码: cebv 

参考文档：RAC的几种IP：https://cloud.tencent.com/developer/article/1578494

# 基本信息与配置

节点基本信息：

注：该编辑文档软件会把用户和密码首字母自动大写，其实为小写。

|              | 节点一                                       | 节点二                                       |
|:------------:|:-----------------------------------------:|:-----------------------------------------:|
| 主机名          | ora1                                      | ora2                                      |
| 操作系统         | Centos7.8                                 | Centos7.8                                 |
| grid用户名/密码   | grid                                      | grid                                      |
| Oracle用户名/密码 | oracle                                    | oracle                                    |
| Oracle 版本    | 11g Enterprise Edition Release 11.2.0.4.0 | 11g Enterprise Edition Release 11.2.0.4.0 |
| oracle用户SID  | rac1                                      | rac2                                      |
| GI安装位置       | /u01/app/grid/base                        | /u01/app/grid/base                        |
| 数据库安装位置      | /u01/app/oracle/base                      | /u01/app/oracle/base                      |
| 公网IP         | 192.168.3.64/21                           | 192.168.3.65/21                           |
| 私网IP         | 192.168.122.2/24                          | 192.168.122.3/24                          |

需要的文件

```bash
# 安装包
p13390677_112040_Linux-x86-64_1of7.zip
p13390677_112040_Linux-x86-64_2of7.zip
p13390677_112040_Linux-x86-64_3of7.zip

# 补丁
p19404309_112040_Linux-x86-64.zip
p18370031_112040_Linux-x86-64.zip

# rpm
pdksh-5.2.14-37.el5_8.1.x86_64.rpm
compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
```

同时挂载本地系统镜像方便yum安装，本文档需要安装的依赖在本地系统镜像中足够安装。

## 用户配置

### gird用户配置

1. 创建用户
   
   ```bash
   groupadd oinstall
   groupadd dba
   groupadd oper
   groupadd asmadmin 
   groupadd asmdba
   groupadd asmoper
   
   useradd -g oinstall -G asmadmin,asmdba,asmoper,dba grid
   echo "grid" | passwd grid --stdin
   ```

2. 编辑Grid用户的 `.bash_profile`
   
   注： ora1的grid用户的ORACLE_SID=+ASM1，ora2的grid用户的ORACLE_SID=+ASM2
   
   ```shell
   export ORACLE_BASE=/u01/app/grid/base
   export ORACLE_HOME=/u01/app/grid/home
   export ORACLE_SID=+ASM1
   export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
   export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
   
   PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
   export PATH
   ```

3. 生效环境变量
   
   ```bash
   . ~/.bash_profile
   ```

### oracle 用户配置

1. 新建oracle用户
   
   ```bash
   useradd -g oinstall -G oper,dba,asmdba -d /home/oracle oracle
   echo "oracle" | passwd oracle --stdin
   ```

2. 编辑oracle用户的`.bash_profile`
   
   注：ora1的ORACLE_SID=rac1，ora2的ORACLE_SID=rac2
   
   ```bash
   export ORACLE_SID=rac1
   export ORACLE_BASE=/u01/app/oracle
   export ORACLE_HOME=/u01/app/oracle/product/11.2.0/db_1
   
   PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
   export PATH
   ```

### 文件夹设置

1. 双库创建相同的文件夹结构(root用户执行)
   
   ```bash
   mkdir -p /u01/app/oracle/product/11.2.0/db_1
   mkdir -p /u01/app/grid/base
   mkdir -p /u01/app/grid/home
   chown -R grid:oinstall /u01
   chown -R oracle:oinstall /u01/app/oracle
   ```

2. 文件放置，zip包自行解压
   
   root家目录下的文件（双节点）
   
   ```
   pdksh-5.2.14-37.el5_8.1.x86_64.rpm
   compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
   ```
   
   grid家目录下的文件（仅节点一）
   
   ```
   p13390677_112040_Linux-x86-64_3of7.zip
   p19404309_112040_Linux-x86-64.zip
   p18370031_112040_Linux-x86-64.zip
   ```
   
   oracle家目录下的文件（仅节点一）
   
   ```
   p13390677_112040_Linux-x86-64_1of7.zip
   p13390677_112040_Linux-x86-64_2of7.zip
   ```

## 系统配置

1. 修改每个节点的/etc/hosts
   
   ```
   192.168.3.64 ora1
   192.168.3.65 ora2
   
   # 以下三个IP交给安装包进行配置，所以安装之前无需ping通以下IP
   192.168.3.88 ora1-vip
   192.168.3.89 ora2-vip
   192.168.3.90 ora-scan
   ```

2. 关闭防火墙和SELinux
   
   ```bash
   systemctl disable firewalld
   systemctl stop firewalld
   # 永久关闭SELINUX
   sed -i 's/=enforcing/=disabled/g'  /etc/selinux/config
   # 或临时关闭：setenforce 0
   # 可选
   reboot
   ```

### 内核参数

1. 修改内核参数文件/etc/sysctl.conf
   
   ```bash
   cat << EOF > /etc/sysctl.conf
   fs.aio-max-nr = 1048576
   fs.file-max = 6815744
   kernel.shmall = 16451328
   kernel.shmmax = 33692319744
   kernel.shmmni = 4096
   kernel.sem = 250 32000 100 128
   net.ipv4.ip_local_port_range = 9000 65500
   net.core.rmem_default = 262144
   net.core.rmem_max = 4194304
   net.core.wmem_default = 262144
   net.core.wmem_max = 1048576
   EOF
   ```

2. 生效内核参数
   
   ```
   sysctl -p
   ```

3. 修改/etc/security/limits.conf 
   
   ```
   cat << EOF > /etc/security/limits.conf
   oracle   soft   nproc    131072
   oracle   hard   nproc    131072
   oracle   soft   nofile   131072
   oracle   hard   nofile   131072
   oracle   soft   core     unlimited
   oracle   hard   core     unlimited
   oracle   soft   memlock  50000000
   oracle   hard   memlock  50000000
   grid soft nproc 2047
   grid hard nproc 16384
   grid soft nofile 1024
   grid hard nofile 65536
   EOF
   ```

4. 在/etc/pam.d/login下添加
   
   ```
   session required pam_limits.so
   ```
   
   这使得用户将加载PAM的pam_limits.so，设置用户登录的硬性设置

### 安装依赖

本地系统镜像中可直接安装(双节点执行)

```
yum install -y binutils-* libc* compat-libstdc++-* elfutils-libelf-* elfutils-libelf-* elfutils-libelf-devel-static-* gcc-* gcc-c++-* glibc-* glibc-common-* glibc-devel-* glibc-headers-* kernel-headers-* libaio-* libaio-devel-* libgcc-* libgomp-* libstdc++-* libstdc++-devel-* make-* sysstat-* compat-libcap* smartmontools
```

额外需要（双节点执行）

```bash
rpm -ivh compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
# 如果 pdksh 与 (已安裝) ksh-20120801-142.el7.x86_64 冲突
# rpm -e ksh-20120801-142.el7.x86_64
rpm -ivh pdksh-5.2.14-37.el5_8.1.x86_64.rpm
```

推荐安装

```bash
yum install -y -q net-tools telnet unzip
```

安装cvuqdisk包（仅节点一）

```bash
# 进入安装文件夹
cd grid/
rpm -ivh rpm/cvuqdisk-1.0.9-1.rpm
```

# 配置共享磁盘

除iscsi服务端外，其他步骤**双节点执行**

参考：[iscsi安装参考文档](https://blog.csdn.net/cuichongxin/article/details/103982207)

## 配置iscsi服务端

1. 安装
   
   ```
   yum install -y target*
   ```

2. 启动并自启
   
   ```
   systemctl enable target
   systemctl start target
   ```

3. 将未格式化、未分区的硬盘进行分区通过`fdisk -l`检查到`/dev/sdb`是我们需要进行共享的磁盘，先将它分区,但不需要格式化
   
   ```
   [root@ora1 grid]# fdisk /dev/sdb
   命令(输入 m 获取帮助)：n
   Select (default p): p
   分区号 (1-4，默认 1)：1
   起始 扇区 (2048-41943039，默认为 2048)：2048
   Last 扇区, +扇区 or +size{K,M,G} (2048-41943039，默认为 41943039)：13981696
   
   命令(输入 m 获取帮助)：n
   Select (default p): p
   分区号 (2-4，默认 2)：2
   起始 扇区 (13981697-41943039，默认为 13983744)：13983744
   Last 扇区, +扇区 or +size{K,M,G} (13983744-41943039，默认为 41943039)：27962026
   
   命令(输入 m 获取帮助)：n
   Select (default p): p
   分区号 (3,4，默认 3)：3
   起始 扇区 (13981697-41943039，默认为 27963392)：27963392
   Last 扇区, +扇区 or +size{K,M,G} (27963392-41943039，默认为 41943039)：41943039
   
   命令(输入 m 获取帮助)：w
   The partition table has been altered!
   Calling ioctl() to re-read partition table.
   正在同步磁盘。
   ```

4. 查看分区
   
   ```
   ora1 ~]# lsblk
   NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda               8:0    0   50G  0 disk 
   ├─sda1            8:1    0    1G  0 part /boot
   └─sda2            8:2    0   49G  0 part 
     ├─centos-root 253:0    0 45.1G  0 lvm  /
     └─centos-swap 253:1    0  3.9G  0 lvm  [SWAP]
   sdb               8:16   0   20G  0 disk 
   ├─sdb1            8:17   0    6G  0 part 
   ├─sdb2            8:18   0    6G  0 part 
   └─sdb3            8:19   0    8G  0 part 
   sr0              11:0    1  4.5G  0 rom  
   loop0             7:0    0  4.5G  1 loop /mnt/iso
   ```

5. 进入子系统
   
   ```
   [root@ora1 ~]# targetcli
   ```

6. 添加硬盘信息格式
   
   ```
   /> /backstores/block create DB1 /dev/sdb1
   /> /backstores/block create DB2 /dev/sdb2
   /> /backstores/block create DB3 /dev/sdb3
   ```

7. 创建iscsi
   
   ```
   格式：/iscsi create 格式.年份-月份.*.com.自定义名称
   /> /iscsi create iqn.2021-06.30.com.oracle
   ```

8. 关联block和target
   
   ```
   /> /iscsi/iqn.2021-06.30.com.oracle/tpg1/luns create /backstores/block/DB1
   /> /iscsi/iqn.2021-06.30.com.oracle/tpg1/luns create /backstores/block/DB2
   /> /iscsi/iqn.2021-06.30.com.oracle/tpg1/luns create /backstores/block/DB3
   ```

9. 创建acl
   
   ```
   /> /iscsi/iqn.2021-06.30.com.oracle/tpg1/acls create iqn.2021-06.30.com.oracle:ora1
   ```

10. 退出
    
    ```
    /> exit
    Global pref auto_save_on_exit=true
    Configuration saved to /etc/target/saveconfig.json
    ```

11. 重启服务
    
    ```
    systemctl restart target
    ```
    
    注：此时会有端口开放
    
    ```
    [root@ora1 ~]# netstat -tunlp |grep 3260
    tcp        0      0 0.0.0.0:3260            0.0.0.0:*               LISTEN      - 
    ```

## 配置iscsi客户端

1. 安装
   
   ```bash
   yum install -y iscsi*
   ```

2. 开机自启
   
   ```bash
   systemctl enable iscsid
   ```

3. 发现target
   
   注：有输出才是被发现，-p 后面的参数是iscsi服务端的ip
   
   ```bash
   [root@ora1 ~]# iscsiadm -m discovery -t st -p 192.168.3.64
   192.168.3.64:3260,1 iqn.2021-06.30.com.oracle
   ```

4. 修改/etc/iscsi/initiatorname.iscsi
   
   ```bash
   InitiatorName=iqn.2021-06.30.com.oracle:ora1
   ```

5. 重启服务
   
   ```bash
   systemctl restart iscsid
   ```

6. 登录
   
   注：需要发现后，才能登陆
   
   ```bash
   iscsiadm -m node -T iqn.2021-06.30.com.oracle -p 192.168.3.64 -l
   ```
   
   如果在主机A格式化了共享盘，在B上其实有了，但可能需要重新登录后，才能在`lsblk`中看见，不登录也可以直接用格式化的磁盘

7. 检查,其中sdc,sdd,sde就是我们共享盘啦
   
   ```bash
   [root@ora1 ~]# lsblk
   NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda               8:0    0   50G  0 disk 
   ├─sda1            8:1    0    1G  0 part /boot
   └─sda2            8:2    0   49G  0 part 
     ├─centos-root 253:0    0 45.1G  0 lvm  /
     └─centos-swap 253:1    0  3.9G  0 lvm  [SWAP]
   sdb               8:16   0   20G  0 disk 
   ├─sdb1            8:17   0    6G  0 part 
   ├─sdb2            8:18   0    6G  0 part 
   └─sdb3            8:19   0    8G  0 part 
   sdc               8:32   0    6G  0 disk 
   sdd               8:48   0    8G  0 disk 
   sde               8:64   0    6G  0 disk 
   sr0              11:0    1  4.5G  0 rom  
   loop0             7:0    0  4.5G  1 loop /mnt/iso
   ```

8. 在节点一分区（），并在节点二重新登录
   
   节点一：
   
   ```bash
   [root@ora1 ~]# fdisk /dev/sdc
   
   命令(输入 m 获取帮助)：n
   Partition type:p
   分区号 (1-4，默认 1)：1
   起始 扇区 (2048-13978965，默认为 2048)：2048
   Last 扇区, +扇区 or +size{K,M,G} (2048-13978965，默认为 13978965)：13978965
   命令(输入 m 获取帮助)：w
   The partition table has been altered!
   正在同步磁盘。
   
   [root@ora1 ~]# fdisk /dev/sdd
   命令(输入 m 获取帮助)：n
   Partition type:
      p   primary (0 primary, 0 extended, 4 free)
      e   extended
   Select (default p): 
   起始 扇区 (2048-13980330，默认为 2048)：
   Last 扇区, +扇区 or +size{K,M,G} (2048-13980330，默认为 13980330)：
   命令(输入 m 获取帮助)：w
   The partition table has been altered!
   正在同步磁盘。
   
   [root@ora1 ~]# fdisk /dev/sde
   命令(输入 m 获取帮助)：n
   Partition type:
   分区号 (1-4，默认 1)：1
   起始 扇区 (2048-13979647，默认为 2048)：2048
   Last 扇区, +扇区 or +size{K,M,G} (2048-13979647，默认为 13979647)：13979647
   命令(输入 m 获取帮助)：w
   The partition table has been altered!
   正在同步磁盘。
   ```
   
   节点二：
   
   ```bash
   # 注销
   [root@ora2 ~]# iscsiadm -m node -T iqn.2021-06.30.com.oracle -p 192.168.3.64 -u
   Logging out of session [sid: 6, target: iqn.2021-06.30.com.oracle, portal: 192.168.3.64,3260]
   Logout of [sid: 6, target: iqn.2021-06.30.com.oracle, portal: 192.168.3.64,3260] successful.
   # 重新登录
   [root@ora2 ~]# iscsiadm -m node -T iqn.2021-06.30.com.oracle -p 192.168.3.64 -l
   Logging in to [iface: default, target: iqn.2021-06.30.com.oracle, portal: 192.168.3.64,3260] (multiple)
   Login to [iface: default, target: iqn.2021-06.30.com.oracle, portal: 192.168.3.64,3260] successful.
   # 可以看到磁盘被分区了
   [root@ora1 ~]# lsblk
   NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda               8:0    0   50G  0 disk 
   ├─sda1            8:1    0    1G  0 part /boot
   └─sda2            8:2    0   49G  0 part 
     ├─centos-root 253:0    0 45.1G  0 lvm  /
     └─centos-swap 253:1    0  3.9G  0 lvm  [SWAP]
   sdb               8:16   0   20G  0 disk 
   ├─sdb1            8:17   0    6G  0 part 
   ├─sdb2            8:18   0    6G  0 part 
   └─sdb3            8:19   0    8G  0 part 
   sdc               8:32   0    6G  0 disk 
   └─sdc1            8:33   0    6G  0 part 
   sdd               8:48   0    8G  0 disk 
   └─sdd1            8:49   0    8G  0 part 
   sde               8:64   0    6G  0 disk 
   └─sde1            8:65   0    6G  0 part 
   sr0              11:0    1  4.5G  0 rom  
   loop0             7:0    0  4.5G  1 loop /mnt/iso
   ```

## 配置多路径

参考文档：

[深入分析Multipath](https://www.huaweicloud.com/articles/14b07cc5cc28d2bc19d3b6330ed05f09.html)

[Oracle rac搭建使用Multipath](https://blog.csdn.net/cuiyan1982/article/details/79082805)

1. 安装软件
   
   ```
   yum install device-mapper* -y
   ```

2. 启用模块
   
   ```
    modprobe dm-multipath
    modprobe dm-round-robin
   ```

3. 查看是否启用
   
   ```bash
   [root@ora1 ~]# lsmod | grep ^dm_m[uo]
   dm_multipath           27792  1 dm_round_robin
   dm_mod                124501  9 dm_multipath,dm_log,dm_mirror
   ```

4. 生成Multipath而配置文件
   
   ```
    mpathconf --enable --find_multipaths y --with_module y --with_multipathd y
   ```

5. 添加共享磁盘的wwid
   
   ```bash
   # 将添加到/etc/multipath/wwid
   [root@ora2 multipath]# multipath -a /dev/sdb
   wwid '3600140579f3599c229346da8e86734e4' added
   [root@ora2 multipath]# multipath -a /dev/sdc
   wwid '36001405faacac4444884193b7de477e6' added
   [root@ora2 multipath]# multipath -a /dev/sdd
   wwid '36001405c8e49d885e6843d9bdc448d26' added
   ```

6. 修改/etc/multipath.conf配置文件
   
   [多路径配置文件详解](https://www.cnblogs.com/azyb/p/10809490.html)
   
   ```
   defaults {
           user_friendly_names yes
           # 启用多路径策略
           path_grouping_policy multibus 
           find_multipaths yes
           path_selector "round-robin 0"
           failback manual
           rr_weight priorities
           no_path_retry 5
   }
   
   # 将本地磁盘排除
   blacklist {
          devnode "^sd[ab]" 
          devnode "sr0"
   }
   multipaths {
          multipath {
                 # 根据multipath -a /dev/命令输出的wwid
                 # 或根据/lib/udev/scsi_id -g -u /dev/sdb 中每个磁盘的WWID来对应
                 wwid 3600140579f3599c229346da8e86734e4
                 alias RAC-OCRVOTE 
          }
          multipath {
                 wwid 36001405faacac4444884193b7de477e6
                 alias RAC-FRA
          }
          multipath {
                 wwid 36001405c8e49d885e6843d9bdc448d26
                 alias RAC-DATA
          }
   }
   ```

7. 重启服务
   
   ```
   systemctl restart multipathd
   ```

8. 检查
   
   此时查看`multipath -v3 | grep "not in wwids file, skipping sdd" `，没有报错。
   
   查看
   
   ```bash
   [root@ora1 ~]# ll /dev/mapper/
   总用量 0
   lrwxrwxrwx 1 root root       7 7月   4 13:13 centos-root -> ../dm-0
   lrwxrwxrwx 1 root root       7 7月   4 13:13 centos-swap -> ../dm-1
   crw------- 1 root root 10, 236 7月   4 13:13 control
   lrwxrwxrwx 1 root root       7 7月   4 13:32 RAC-DATA -> ../dm-4
   lrwxrwxrwx 1 root root       7 7月   4 13:32 RAC-DATA1 -> ../dm-7
   lrwxrwxrwx 1 root root       7 7月   4 13:32 RAC-FRA -> ../dm-2
   lrwxrwxrwx 1 root root       7 7月   4 13:32 RAC-FRA1 -> ../dm-5
   lrwxrwxrwx 1 root root       7 7月   4 13:32 RAC-OCRVOTE -> ../dm-3
   lrwxrwxrwx 1 root root       7 7月   4 13:32 RAC-OCRVOTE1 -> ../dm-6
   ```
   
   查看
   
   ```bash
   [root@ora1 ~]# multipathd show paths
   hcil    dev dev_t pri dm_st  chk_st dev_st  next_check     
   3:0:0:2 sdd 8:48  1   active ready  running .......... 1/20
   3:0:0:0 sdc 8:32  1   active ready  running .......... 1/20
   3:0:0:1 sde 8:64  1   active ready  running .......... 1/20
   ```

   到此multipath 配置完成！

## 配置udev

参考文档：[udev简介及配置](http://blog.itpub.net/25469263/viewspace-2156439/)

通过UDEV配置，能够让Oracle对磁盘名进行持久化并改变磁盘访问权限为grid:asmadmin。这样在ASM的配置过程中能够看到磁盘。今后增加磁盘也不会改变原有的磁盘名称。该步骤可使用ASMLIB

1. 根据共享盘查看dm_uuid
   
   ```bash
    [root@ora1 ~]# for i in RAC-OCRVOTE1 RAC-DATA1 RAC-FRA1; do
    printf "%s %s\n" "$i" "$(udevadm info --query=all --name=/dev/mapper/$i |grep -i -o dm_uuid.*)"; 
    done
   
   RAC-OCRVOTE1 DM_UUID=part1-mpath-3600140579f3599c229346da8e86734e4
   RAC-DATA1 DM_UUID=part1-mpath-36001405c8e49d885e6843d9bdc448d26
   RAC-FRA1 DM_UUID=part1-mpath-36001405faacac4444884193b7de477e6
   ```

2. 添加udev规则
   
   编辑/etc/udev/rules.d/99-oracle-asmdevices.rules
   
   ENV{DM_UUID}==""依次为上诉查找的结果，SYMLINK（该名称作为/dev/.....）依次为**asm-ocrvote**，**asm-data**，**asm-fra**
   
   ```bash
   # 模板
   # KERNEL=="dm-*",SYMLINK+="",ENV{DM_UUID}=="",OWNER="grid",GROUP="asmadmin",MODE="0660"
   KERNEL=="dm-*",ENV{DM_UUID}=="part1-mpath-3600140579f3599c229346da8e86734e4",SYMLINK+="asm-ocrvote",OWNER="grid",GROUP="asmadmin",MODE="0660"
   KERNEL=="dm-*",ENV{DM_UUID}=="part1-mpath-36001405c8e49d885e6843d9bdc448d26",SYMLINK+="asm-data",OWNER="grid",GROUP="asmadmin",MODE="0660"
   KERNEL=="dm-*",ENV{DM_UUID}=="part1-mpath-36001405faacac4444884193b7de477e6",SYMLINK+="asm-fra",OWNER="grid",GROUP="asmadmin",MODE="0660"
   ```

3. 双节点添加完规则后,重启udev服务
   
   ```bash
   udevadm control --reload-rules
   udevadm trigger --type=devices --action=change
   ```

4. 测试文件是否生效
   
   在查看`ll /dev/mapper/`时，能够发现`RAC-FRA1 -> ../dm-5`，`RAC-OCRVOTE1 -> ../dm-6`，`RAC-DATA1 -> ../dm-7`，针对dm-5、dm-6、dm-7进行查看。
   
   ```
   udevadm test /sys/block/dm-5
   ```
   
   查看权限分配是否正确
   
   ```bash
   [root@ora1 rules.d]# ll /dev/dm-5
   brw-rw---- 1 grid asmadmin 253, 5 7月   4 14:06 /dev/dm-5
   ```

# 安装Grid Infrastructure

标题无特殊特殊说明的步骤，均使用grid用户执行

## 打补丁19404309

备份文件并覆盖文件

```
[grid@ora1 ~]$ cp grid/stage/cvu/cvu_prereq.xml !#:1.save 
cp grid/stage/cvu/cvu_prereq.xml grid/stage/cvu/cvu_prereq.xml.save
[grid@ora1 ~]$ cp b19404309/grid/cvu_prereq.xml grid/stage/cvu/cvu_prereq.xml
```

## grid用户互信

配置RAC的节点互信，在主节点执行一次即可

```bash
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user grid -advanced -noPromptPassphrase
```

注：验证互信的方式：`ssh 主机名 date`，能免密执行

### 配置GI

1. 执行检查
   
   ```bash
   ./runcluvfy.sh stage -pre crsinst -n ora1,ora2 -fixup -verbose
   ```

2. 修改response/grid_install.rsp
   
   注：主节点修改该文件即可，其他节点将被复制安装文件
   
   ```ini
   oracle.install.responseFileVersion=/oracle/install/rspfmt_crsinstall_response_schema_v11_2_0
   # 主机名
   ORACLE_HOSTNAME=ora1
   # INVENTORY 目录
   INVENTORY_LOCATION=/u01/app/grid/oraInventory
   SELECTED_LANGUAGES=en
   oracle.install.option=CRS_CONFIG
   # ORACLE_HOME:不能在ORACLE_BASE之下且不相同
   ORACLE_BASE=/u01/app/grid/base
   ORACLE_HOME=/u01/app/grid/home
   oracle.install.asm.OSDBA=asmdba
   oracle.install.asm.OSOPER=asmoper
   oracle.install.asm.OSASM=asmadmin
   # scanName:scan ip
   oracle.install.crs.config.gpnp.scanName=ora-scan
   oracle.install.crs.config.gpnp.scanPort=1521
   # cluster Name:集群名
   oracle.install.crs.config.clusterName=rac
   oracle.install.crs.config.gpnp.configureGNS=false
   oracle.install.crs.config.gpnp.gnsSubDomain=
   oracle.install.crs.config.gpnp.gnsVIPAddress=
   oracle.install.crs.config.autoConfigureClusterNodeVIP=false
   # cluster node:前者公网，后者VIP
   oracle.install.crs.config.clusterNodes=ora1:ora1-vip,ora2:ora2-vip
   # networkInterfaceList:填写网关,前者公网，后者私网，均为真实网卡，填写该IP的网段号，接口3是未被使用
   oracle.install.crs.config.networkInterfaceList=eth0:192.168.0.0:1,eth5:192.168.122.0:2
   oracle.install.crs.config.storageOption=ASM_STORAGE
   oracle.install.crs.config.sharedFileSystemStorage.diskDriveMapping=
   oracle.install.crs.config.sharedFileSystemStorage.votingDiskLocations=
   oracle.install.crs.config.sharedFileSystemStorage.votingDiskRedundancy=NORMAL
   oracle.install.crs.config.sharedFileSystemStorage.ocrLocations=
   oracle.install.crs.config.sharedFileSystemStorage.ocrRedundancy=NORMAL
   oracle.install.crs.config.useIPMI=false
   oracle.install.crs.config.ipmi.bmcUsername=
   oracle.install.crs.config.ipmi.bmcPassword=
   oracle.install.asm.SYSASMPassword=Oracle123
   # 表决盘
   oracle.install.asm.diskGroup.name=OCRVOTE
   # NORMAL模式需要指定三块，EXTERNAL模式只需要制定一块盘
   # 剩下2块盘，留作创建磁盘组
   oracle.install.asm.diskGroup.redundancy=EXTERNAL
   oracle.install.asm.diskGroup.AUSize=1
   # 指定表决盘盘路径,根据ll /dev/RAC-ORAVOTE指向的路径
   oracle.install.asm.diskGroup.disks=/dev/dm-7
   # 发现其他asm磁盘
   oracle.install.asm.diskGroup.diskDiscoveryString=/dev/dm-*
   # password
   oracle.install.asm.monitorPassword=Oracle123
   oracle.install.crs.upgrade.clusterNodes=
   oracle.install.asm.upgradeASM=false
   oracle.installer.autoupdates.option=SKIP_UPDATES
   oracle.installer.autoupdates.downloadUpdatesLoc=
   AUTOUPDATES_MYORACLESUPPORT_USERNAME=
   AUTOUPDATES_MYORACLESUPPORT_PASSWORD=
   PROXY_HOST=
   PROXY_PORT=0
   PROXY_USER=
   PROXY_PWD=
   PROXY_REALM=
   ```

3. 进入安装文件夹，并执行下列命令，开始静默安装（约十分钟）
   
   ```bash
   ./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/grid_install.rsp  -showprogress
   ```
   
   注：图形化安装窗口异常的问题解决命令：`./runInstaller -jreLoc /etc/alternatives/jre_1.8.0`
   
   注：通过`echo $?`是否返回`0`来判断安装成功，一般出现`CAUSE: `就是失败了

## 执行root.sh前的补丁p18370031

该脚本针对**CentOS7**的服务

### 没有补丁的解决方案

不打补丁时，执行root.sh将提示**Adding Clusterware entries to inittab**

1. 编写启动脚本
   
   ```bash
   cat << EOF > /usr/lib/systemd/system/ohasd.service
   [Unit]
   Description=Oracle High Availability Services
   After=syslog.target
   [Service]
   ExecStart=/etc/init.d/init.ohasd run >/dev/null 2>&1 Type=simple
   Restart=always
   [Install]
   WantedBy=multi-user.target
   EOF
   ```

2. 修改服务
   
   ```bash
   chmod 755 /usr/lib/systemd/system/ohasd.service
   systemctl daemon-reload
   systemctl enable ohasd.service
   ```

3. 在运行root.sh时，`/etc/init.d/`目录出现`inint.ohasd`时启动服务
   
   ```
   systemctl start ohasd.service
   ```

### 有补丁的解决方案

打补丁后，执行root.sh将提示**Adding Clusterware entries to oracle-ohasd.service**

注：节点一打补丁后，根据可以选择是否在其他节点上执行

1. 进入补丁目录
   
   ```bash
   cd 18370031/
   ```

2. 安装补丁
   
   安装过程中
   
   ```
   /u01/app/grid/home/OPatch/opatch apply
   ```

3. 检查安装历史记录
   
   ```
   /u01/app/grid/home/OPatch/opatch lshistory
   ```

## 执行脚本（root用户执行）

1. 以root用户执行
   
   注：第一个节点执行完俩条命令后，再去二个节点执行，务必双节点执行。否则安装数据库找不到没有执行root.sh的主机
   
   注，出错时多检查日志，执行完毕后可`ehco $?`查看返回值，0则正确
   
   ```bash
   /u01/app/grid/oraInventory/orainstRoot.sh
   # 约十分钟
   /u01/app/grid/home/root.sh
   ```
   
   完美执行脚本Root.sh 日志如下，仅供参考
   
   ```
   Creating /etc/oratab file...
   Entries will be added to the /etc/oratab file as needed by
   Database Configuration Assistant when a database is created
   Finished running generic part of root script.
   Now product-specific root actions will be performed.
   Using configuration parameter file: /u01/app/grid/home/crs/install/crsconfig_params
   Creating trace directory
   User ignored Prerequisites during installation
   Installing Trace File Analyzer
   OLR initialization - successful
     root wallet
     root wallet cert
     root cert export
     peer wallet
     profile reader wallet
     pa wallet
     peer wallet keys
     pa wallet keys
     peer cert request
     pa cert request
     peer cert
     pa cert
     peer root cert TP
     profile reader root cert TP
     pa root cert TP
     peer pa cert TP
     pa peer cert TP
     profile reader pa cert TP
     profile reader peer cert TP
     peer user cert
     pa user cert
   Adding Clusterware entries to oracle-ohasd.service
   CRS-2672: Attempting to start 'ora.mdnsd' on 'ora1'
   CRS-2676: Start of 'ora.mdnsd' on 'ora1' succeeded
   CRS-2672: Attempting to start 'ora.gpnpd' on 'ora1'
   CRS-2676: Start of 'ora.gpnpd' on 'ora1' succeeded
   CRS-2672: Attempting to start 'ora.cssdmonitor' on 'ora1'
   CRS-2672: Attempting to start 'ora.gipcd' on 'ora1'
   CRS-2676: Start of 'ora.cssdmonitor' on 'ora1' succeeded
   CRS-2676: Start of 'ora.gipcd' on 'ora1' succeeded
   CRS-2672: Attempting to start 'ora.cssd' on 'ora1'
   CRS-2672: Attempting to start 'ora.diskmon' on 'ora1'
   CRS-2676: Start of 'ora.diskmon' on 'ora1' succeeded
   CRS-2676: Start of 'ora.cssd' on 'ora1' succeeded
   
   已成功创建并启动 ASM。
   
   已成功创建磁盘组OCRVOTE。
   
   clscfg: -install mode specified
   Successfully accumulated necessary OCR keys.
   Creating OCR keys for user 'root', privgrp 'root'..
   Operation successful.
   CRS-4256: Updating the profile
   Successful addition of voting disk 214e1a35dd694f02bf891fa4259699f3.
   Successfully replaced voting disk group with +OCRVOTE.
   CRS-4256: Updating the profile
   CRS-4266: Voting file(s) successfully replaced
   ##  STATE    File Universal Id                File Name Disk group
   --  -----    -----------------                --------- ---------
    1. ONLINE   214e1a35dd694f02bf891fa4259699f3 (/dev/dm-7) [OCRVOTE]
   Located 1 voting disk(s).
   CRS-2672: Attempting to start 'ora.asm' on 'ora1'
   CRS-2676: Start of 'ora.asm' on 'ora1' succeeded
   CRS-2672: Attempting to start 'ora.OCRVOTE.dg' on 'ora1'
   CRS-2676: Start of 'ora.OCRVOTE.dg' on 'ora1' succeeded
   软件包准备中...
   cvuqdisk-1.0.9-1.x86_64
   Configure Oracle Grid Infrastructure for a Cluster ... succeeded
   ```

2. 在主节点以grid用户执行
   
   ```bash
   /u01/app/grid/home/cfgtoollogs/configToolAllCommands RESPONSE_FILE=<response_file>
   ```
   
   注：response_file请使用绝对路径
   
   注：plug-in Automatic Storage Management Configuration Assistan失败关系不大($?返回3)
   
   注：如果不执行该语句，安装数据库时会提示没有安装GI，因此务必执行。

# 安装并创建数据库

## 安装oracle软件(oracle用户执行)

1. oracle用户互信
   
   ```bash
   # 进入安装目录
   cd database
   sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user oracle -advanced -noPromptPassphrase
   ```
   
   注：互信检查方式为：`ssh 主机名 date`,能免密执行

2. 资源文件操作
   
   备份资源文件
   
   ```bash
   cp response/db_install.rsp response/db_install.rsp.save
   ```
   
   修改response/db_install.rsp
   
   ```ini
   oracle.install.responseFileVersion=/oracle/install/rspfmt_dbinstall_response_schema_v11_2_0
   oracle.install.option=INSTALL_DB_SWONLY
   # hostname
   ORACLE_HOSTNAME=ora1
   UNIX_GROUP_NAME=oinstall
   INVENTORY_LOCATION=/u01/app/oracle/Inventory
   SELECTED_LANGUAGES=en
   # ORACLE_HOME
   ORACLE_HOME=/u01/app/oracle/product/11.2.0/db_1
   ORACLE_BASE=/u01/app/oracle
   oracle.install.db.InstallEdition=EE
   oracle.install.db.EEOptionsSelection=false
   oracle.install.db.optionalComponents=oracle.rdbms.partitioning:11.2.0.4.0,oracle.oraolap:11.2.0.4.0,oracle.rdbms.dm:11.2.0.4.0,oracle.rdbms.dv:11.2.0.4.0,o
   oracle.install.db.DBA_GROUP=dba
   oracle.install.db.OPER_GROUP=dba
   # node
   oracle.install.db.CLUSTER_NODES=ora1,ora2
   oracle.install.db.isRACOneInstall=false
   oracle.install.db.racOneServiceName=
   oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
   # globalDBName
   oracle.install.db.config.starterdb.globalDBName=racdb
   oracle.install.db.config.starterdb.SID=rac
   oracle.install.db.config.starterdb.characterSet=AL32UTF8
   oracle.install.db.config.starterdb.memoryOption=false
   oracle.install.db.config.starterdb.memoryLimit=
   oracle.install.db.config.starterdb.installExampleSchemas=false
   oracle.install.db.config.starterdb.enableSecuritySettings=false
   oracle.install.db.config.starterdb.password.ALL=Oracle#123
   oracle.install.db.config.starterdb.password.SYS=
   oracle.install.db.config.starterdb.password.SYSTEM=
   oracle.install.db.config.starterdb.password.SYSMAN=
   oracle.install.db.config.starterdb.password.DBSNMP=
   oracle.install.db.config.starterdb.control=DB_CONTROL
   oracle.install.db.config.starterdb.gridcontrol.gridControlServiceURL=
   oracle.install.db.config.starterdb.automatedBackup.enable=false
   oracle.install.db.config.starterdb.automatedBackup.osuid=
   oracle.install.db.config.starterdb.automatedBackup.ospwd=
   oracle.install.db.config.starterdb.storageType=
   oracle.install.db.config.starterdb.fileSystemStorage.dataLocation=
   oracle.install.db.config.starterdb.fileSystemStorage.recoveryLocation=
   oracle.install.db.config.asm.diskGroup=
   oracle.install.db.config.asm.ASMSNMPPassword=Oracle#123
   MYORACLESUPPORT_USERNAME=
   MYORACLESUPPORT_PASSWORD=
   SECURITY_UPDATES_VIA_MYORACLESUPPORT=false
   DECLINE_SECURITY_UPDATES=true
   PROXY_HOST=
   PROXY_PORT=
   PROXY_USER=
   PROXY_PWD=
   PROXY_REALM=
   COLLECTOR_SUPPORTHUB_URL=
   oracle.installer.autoupdates.option=SKIP_UPDATES
   oracle.installer.autoupdates.downloadUpdatesLoc=
   AUTOUPDATES_MYORACLESUPPORT_USERNAME=
   AUTOUPDATES_MYORACLESUPPORT_PASSWORD=
   ```

3. 节点1执行进行安装
   
   ```bash
   ./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/db_install.rsp -showProgress
   ```
   
   注：每个节点需要有相同文件结构(已实施)
   
   注：其他远程节点会自动复制，远程节点的$ORACLE_HOME大约有4.2G，可查看进度（大约十五分钟）
   
   注：安装之间再检查下oracle用户是否属于dba组，否则安装失败，会提示` CAUSE: The installation user account must be a member of all groups required for installation.`
   
   注：不要输出`........94% Done`和`Setup files successful.`作为安装结束的标志，多等一会。。每个节点的文件大小基本相同，刚出这两条日志时，节点二还没达到4.2G的大小

4. 使用root用户执行root.sh脚本
   
   ```
   /u01/app/oracle/product/11.2.0/db_1/root.sh
   ```

## 创建磁盘组(grid用户)

1. 以grid用户进入SQLPLUS，其中==ORALCE_SID=+ASM1==
   
   ```
    sqlplus / as sysasm
   ```

2. 查看当前磁盘组
   
   ```sql
    SQL> select group_number,name,state,total_mb,free_mb from v$asm_diskgroup;
   GROUP_NUMBER NAME                                  STATE            TOTAL_MB    FREE_MB
   ------------ ------------------------------ ----------- ---------- ----------
                1 OCRVOTE                              MOUNTED              6140         5744
   ```

3. 创建磁盘组
   
   ```sql
   SQL> CREATE DISKGROUP DATA external REDUNDANCY disk '/dev/dm-6' ATTRIBUTE 'au_size'='1M','compatible.asm'='11.2';
   Diskgroup created.
   
   SQL> CREATE DISKGROUP FRA external REDUNDANCY disk '/dev/dm-5' ATTRIBUTE 'au_size'='1M','compatible.asm'='11.2';
    Diskgroup created.
   ```

4. 再次查看当前磁盘组
   
   ```sql
   SQL> select group_number,name,state,total_mb,free_mb from v$asm_diskgroup;
   
   GROUP_NUMBER NAME                                  STATE            TOTAL_MB    FREE_MB
   ------------ ------------------------------ ----------- ---------- ----------
                1 OCRVOTE                              MOUNTED              6140         5744
                2 DATA                                  MOUNTED              6140         6088
                3 FRA                                  MOUNTED              8187         8135
   ```
   
   注：另一节点上也可看到创建的磁盘组

## 安装数据库（oracle用户）

1. 备份资源文件
   
   ```bash
   cd $ORACLE_HOME
   cp assistants/dbca/dbca.rsp !#:1.save
   ```

2. 修改$ORACLE_HOME/assistants/dbca/dbca.rsp
   
   ```ini
   [CREATEDATABASE]
   GDBNAME = "racdb"
   templateName=General_Purpose.dbc
   # 该文件SID指定的是前缀
   # 所以oracle的用户的ORACLE_SID应该是rac1与rac2
   SID = "rac"
   NODELIST="ora1,ora2"
   SYSPASSWORD = "password"
   SYSTEMPASSWORD = "password"
   SYSMANPASSWORD = "password"
   DBSNMPPASSWORD = "password"
   DATAFILEDESTINATION ="+DATA"
   RECOVERYAREADESTINATION="+DATA"
   STORAGETYPE="ASM"
   CHARACTERSET = "AL32UTF"
   NATIONALCHARACTERSET= "UTF8"
   DISKGROUPNAME=DATA
   OBFUSCATEDPASSWORDS=FALSE
   SAMPLESCHEMA=FALSE
   ```

3. 使用dbca安装数据库
   
   ```bash
   dbca -silent -createDatabase -responseFile `pwd`/assistants/dbca/dbca.rsp
   ```
   
   注：如果$?不是0，可以去/u01/app/oracle/cfgtoollogs/dbca查看日志
   
   注：当节点一执行后，可在节点二的oracle用户（==ORALCE_SID=rac2==）实时查看新建的数据库

# 结束

到此安装结束！下面是部分命令

- 检查RAC状态
  
  ```bash
  [grid@ora1 ~]$ crs_stat -t
  Name           Type           Target    State     Host        
  ------------------------------------------------------------
  ora.DATA.dg    ora....up.type ONLINE    ONLINE    ora1        
  ora....ER.lsnr ora....er.type ONLINE    ONLINE    ora1        
  ora....N1.lsnr ora....er.type ONLINE    ONLINE    ora2        
  ora....N2.lsnr ora....er.type ONLINE    ONLINE    ora1        
  ora....N3.lsnr ora....er.type ONLINE    ONLINE    ora1        
  ora.asm        ora.asm.type   ONLINE    ONLINE    ora1        
  ora.cvu        ora.cvu.type   ONLINE    ONLINE    ora1        
  ora.gsd        ora.gsd.type   OFFLINE   OFFLINE    
  ora....network ora....rk.type ONLINE    ONLINE    ora1        
  ora.oc4j       ora.oc4j.type  ONLINE    ONLINE    ora1        
  ora.ons        ora.ons.type   ONLINE    ONLINE    ora1        
  ora....SM1.asm application    ONLINE    ONLINE    ora1        
  ora....A1.lsnr application    ONLINE    ONLINE    ora1        
  ora.ora1.gsd   application    OFFLINE   OFFLINE               
  ora.ora1.ons   application    ONLINE    ONLINE    ora1        
  ora.ora1.vip   ora....t1.type ONLINE    ONLINE    ora1        
  ora....SM2.asm application    ONLINE    ONLINE    ora2        
  ora....A2.lsnr application    ONLINE    ONLINE    ora2        
  ora.ora2.gsd   application    OFFLINE   OFFLINE               
  ora.ora2.ons   application    ONLINE    ONLINE    ora2        
  ora.ora2.vip   ora....t1.type ONLINE    ONLINE    ora2        
  ora.scan1.vip  ora....ip.type ONLINE    ONLINE    ora2        
  ora.scan2.vip  ora....ip.type ONLINE    ONLINE    ora1        
  ora.scan3.vip  ora....ip.type ONLINE    ONLINE    ora1
  
  [grid@ora1 ~]$ crsctl check crs
  CRS-4638: Oracle High Availability Services is online
  CRS-4537: Cluster Ready Services is online
  CRS-4529: Cluster Synchronization Services is online
  CRS-4533: Event Manager is online
  ```
  
  注：gsd结尾项状态为OFFLINE是正常
  
  注：启动/关闭集群需要root权限，可以把grid用户变量复制到root用户的.bash_profile下

- 用root用户直接关闭并启动集群
  
  ```
  # 关得快
  [root@ora1 ~]# crsctl stop cluster -all
  # 启动慢
  [root@ora1 ~]# crsctl start cluster -all
  ```
