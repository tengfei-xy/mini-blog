## 环境

CentOS7/oracle 11g

## 前提需要

### 补丁

```
p19404309_112040_Linux-x86-64.zip
p18370031_112040_Linux-x86-64.zip
```

## 准备工作

```bash
vi /etc/security/limits.conf 
grid    hard    nofile   65536
grid    hard    nproc   16384
```



```shell
unzip p13390677_112040_Linux-x86-64_3of7.zip
```

### 安装依赖

```bash
yum install -y binutils-* libc* compat-libstdc++-* elfutils-libelf-* elfutils-libelf-* elfutils-libelf-devel-static-* gcc-* gcc-c++-* glibc-* glibc-common-* glibc-devel-* glibc-headers-* kernel-headers-* ksh-* libaio-* libaio-devel-* libgcc-* libgomp-* libstdc++-* libstdc++-devel-* make-* sysstat-* compat-libcap* 
```

### 安装cvuqdisk包

```bash
yum install -y smartmontools
rpm -ivh rpm/cvuqdisk-1.0.9-1.rpm
```

### 安装asmlib驱动

```bash
rpm -ivh kernel-3.10.0-1160.11.1.el7.x86_64.rpm kernel-3.10.0-1160.15.2.el7.x86_64.rpm
 rpm -ivh kmod-oracleasm-2.0.8-28.el7.x86_64.rpm oracleasmlib-2.0.12-1.el7.x86_64.rpm oracleasm-support-2.1.11-2.el7.x86_64.rpm
```



## ssh 相互通信

### 配置grid的节点互信，在一个节点用grid用户执行

```bash
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user grid -advanced -noPromptPassphrase
```

### 配置oracle的节点互信，在一个节点用oracle用户执行

```bash
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user oracle -advanced -noPromptPassphrase
```



### 执行grid安装检查

```bash
su - grid
cd grid
./runcluvfy.sh stage -pre crsinst -n ora1,ora2 -fixup -verbose
```

### RAC 用户属组

```bash
groupadd oinstall
groupadd dba
groupadd oper
groupadd asmadmin 
groupadd asmdba
groupadd asmoper

useradd -g oinstall -G oper,dba,asmdba -d /home/oracle oracle
useradd -g oinstall -G asmadmin,asmdba,asmoper,dba grid
echo "grid" | passwd grid --stdin
#usermod -g oinstall -G oper,dba,asmdba oracle
```

### .bash_profile

```shell
export ORACLE_BASE=/grid/base
export ORACLE_HOME=/grid/home
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
export PATH
```

## 打补丁：p19404309

```bash
unzip p19404309_112040_Linux-x86-64.zip
# 将补丁包中的cvu_prereq.xml 复制到 安装包stage/cvu/下
cp b19404309/grid/cvu_prereq.xml stage/cvu/
```





## 安装grid

```bash
./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/grid_install.rsp  -showprogress
```

### grid_install.rsp

```ini
oracle.install.responseFileVersion=/oracle/install/rspfmt_crsinstall_response_schema_v11_2_0
# 主机名
ORACLE_HOSTNAME=ora1
INVENTORY_LOCATION=/data/app/grid/Inventory
SELECTED_LANGUAGES=en
oracle.install.option=CRS_CONFIG
# 基本变量 ORACLE_HOME不能在ORACLE_BASE之下且不相同
ORACLE_BASE=/data/app/grid/base
ORACLE_HOME=/data/app/grid/home

oracle.install.asm.OSDBA=asmdba
oracle.install.asm.OSOPER=asmoper
oracle.install.asm.OSASM=asmadmin
# scan ip
oracle.install.crs.config.gpnp.scanName=ora-scan
oracle.install.crs.config.gpnp.scanPort=1521
# cluster Name
oracle.install.crs.config.clusterName=orclerac
oracle.install.crs.config.gpnp.configureGNS=false
oracle.install.crs.config.gpnp.gnsSubDomain=
oracle.install.crs.config.gpnp.gnsVIPAddress=
oracle.install.crs.config.autoConfigureClusterNodeVIP=false
# cluster node 
oracle.install.crs.config.clusterNodes=ora2:ora2-vip,ora1:ora1-vip
# network interface list 需要三个
oracle.install.crs.config.networkInterfaceList=ens33:192.168.245.5:1,ens37:192.168.245.8:2,ens33:192.168.245.8:3
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
oracle.install.asm.diskGroup.name=OCR
oracle.install.asm.diskGroup.redundancy=NORMAL
oracle.install.asm.diskGroup.AUSize=1
# 至少三块
oracle.install.asm.diskGroup.disks=/dev/oracleasm/disks/DB1,/dev/oracleasm/disks/DB2,/dev/oracleasm/disks/DB3
oracle.install.asm.diskGroup.diskDiscoveryString=/dev/oracleasm/disks/*
oracle.install.asm.monitorPassword=Oracle123
oracle.install.crs.upgrade.clusterNodes=
oracle.install.asm.upgradeASM=false
oracle.installer.autoupdates.option=SKIP_UPDATES
oracle.installer.autoupdates.downloadUpdatesLoc=
AUTOUPDATES_MYORACLESUPPORT_USERNAME=
AUTOUPDATES_MYORACLESUPPORT_PASSWORD=
PROXY_HOST=
PROXY_PORT=
PROXY_USER=
PROXY_PWD=
PROXY_REALM=

```

### 以安装用户执行

```bash
/grid/home/cfgtoollogs/configToolAllCommands RESPONSE_FILE=`pwd`/response/grid_install.rsp
```

## 执行root