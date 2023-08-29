# 19c-shell

## 目录

-   [Centos7 shell安装19c rac](#Centos7-shell安装19c-rac)
    -   [参考文档](#参考文档)
    -   [环境准备](#环境准备)
    -   [脚本使用步骤](#脚本使用步骤)
    -   [问题列表](#问题列表)
    -   [脚本bug](#脚本bug)

# Centos7 shell安装19c rac

注：如果你没有成功手动部署过rac，请出门左转

## 参考文档

本文使用的OracleShellInstall.sh的md5为

```纯文本
6f1b731204f71a18b00263214385f60d
```

## 环境准备

双节点（Centos7.6），每个节点需要

1.  有两张网卡（作为公网IP和私网IP）
2.  共享盘就绪
3.  CD系统盘连接

## 脚本使用步骤

安装依赖包

```纯文本
yum -y -q install bc gcc gcc-c++  binutils  make gdb cmake  glibc ksh elfutils-libelf elfutils-libelf-devel fontconfig-devel glibc-devel libaio libXrender libXrender-devel libX11 libXau sysstat libXi libXtst libgcc librdmacm-devel libstdc++ libstdc++-devel libxcb net-tools nfs-utils compat-libcap1 smartmontools  targetcli python python-configshell python-rtslib python-six  unixODBC unixODBC-devel iscsi-initiator-utils unbound bind-utils kde-l10n-Chinese.noarch xclock lsof expect unzip redhat-lsb telnep ntsysv strace dos2unix ncurses-devel
```

以及额外依赖包

```纯文本
compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
libaio-devel-0.3.109-13.el7.x86_64.rpm
```

定义初始化变量（三个表决盘、一个数据盘的非CDB模式的参数）

```bash
i=172.16.83.130     # node1 Public ip(本机执行脚本的IP)
n=ora               # rac hostname prefix
c=true              # 判断是否为CDB模式
pb=rac              # 创建PDB的名称
o=rac               # oraclesid
rs=tengfei          # root password
op=oracle           # oracle password
gp=grid             # grid password
b=/u01/app          # install basedir
s=AL32UTF8          # characterset
ns=UTF8             # national characterset
pb1=$i              # node1 public ip
pb2=172.16.83.131   # node2 public ip
vi1=172.16.83.132   # node1 virtual ip
vi2=172.16.83.133   # node2 virtual ip
pi1=192.168.207.133 # node1 private ip
pi2=192.168.207.149 # node2 private ip
si=172.16.83.134    # scan ip
od="/dev/sdc,/dev/sde,/dev/sdf" # asm ocr disk
dd="/dev/sdd"       # asm data disk
on=OCR              # asm ocr diskgroupname
dn=DATA             # asm data diskgroupname
or=NORMAL           # asm ocr redundancy
dr=EXTERNAL         # asm data redundancy
puf=ens33           # 主机的公网IP对应的网卡名称
prf=ens36           # 主机的私有IP对应的网卡名称
iso=y               # 配置yum源
```

执行脚本

```bash
./OracleShellInstall.sh -i $i -n $n -c $c -pb $pb -o $o -rs $rs -op $op -gp $gp -b $b -s $s -ns $ns -pb1 $pb1 -pb2 $pb2 -vi1 $vi1 -vi2 $vi2 -pi1 $pi1 -pi2 $pi2 -si $si -od $od -dd $dd -on $on -dn $dn -or $or -dr $dr -puf $puf -prf $prf -iso $iso
```

## 问题列表

-   gi、oracle的安装包和OracleShellInstall.sh放在/soft或/tmp下，请勿放到root下
-   安装包真实存在,但提示找不到包时
    ```纯文本
    rm -rf /u01
    ```
-   gi安装失败是因ocr或data的初始参数设置错误时，需要删除如下内容，调整初始参数后重新执行脚本
    ```纯文本
    rm -f /etc/udev/rules.d/99-oracle-asmdevices.rules
    rm -f /etc/multipath.conf
    rm -rf /u01
    ```

## 脚本bug

在未经过任何修改的脚本中，存在如下问题

1.  grep过滤的关键词包含多个iso时会出错
    默认（864行左右）：
    ```纯文本
    mountPatch=$(mount | grep -E "iso|ISO" | awk '{print $3}')
    ```
    改为：
    ```纯文本
    mountPatch=$(mount -l | grep -i "iso9660" | awk '{print $3}')
    ```
2.  当根目录挂载于/dev/vda时，iscsi的共享磁盘就会从sda开始,此时/dev/sda不能加入黑名单
    默认（1603行左右）
    ```纯文本
    devnode "^sda"
    ```
    改为：
    ```纯文本
    #devnode "^sda"
    ```
