# Centos7静默安装oracle 11g rac

## 准备工作

### 关闭防火墙和SELinux

```bash
systemctl disable firewalld
# 永久关闭SELINUX
sed -i 's/=enforcing/=disabled/g'  /etc/selinux/config
reboot
```

### 修改内核参数

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
sysctl -p
```

### 修改用户参数

```bash
vi /etc/security/limits.conf 
grid soft nproc 2047
grid hard nproc 16384
grid soft nofile 1024
grid hard nofile 65536
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

## gird 用户设置

```bash
groupadd oinstall
groupadd dba
groupadd oper
groupadd asmadmin 
groupadd asmdba
groupadd asmoper

useradd -g oinstall -G asmadmin,asmdba,asmoper,dba grid
echo "grid" | passwd grid --stdin
#usermod -g oinstall -G oper,dba,asmdba oracle
```

### .bash_profile

```shell
export ORACLE_BASE=/data/app/grid/base
export ORACLE_HOME=/data/app/grid/home
export ORACLE_SID=+ASM1
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
export PATH
```

### 配置grid的节点互信，在主节点用grid用户执行

```bash
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user grid -advanced -noPromptPassphrase
```

## 安装共享磁盘

```bash
fdisk /dev/sdc
fdisk /dev/sdd
fdisk /dev/sde
chwon -R grid:oinstall /dev/sdc1
chown -R grid:oinstall /dev/sdc1
chown -R grid:oinstall /dev/sdd1
chown -R grid:oinstall /dev/sde1
```

### 安装asmlib驱动

```bash
rpm -ivh kernel-3.10.0-1160.11.1.el7.x86_64.rpm kernel-3.10.0-1160.15.2.el7.x86_64.rpm
rpm -ivh kmod-oracleasm-2.0.8-28.el7.x86_64.rpm oracleasmlib-2.0.12-1.el7.x86_64.rpm oracleasm-support-2.1.11-2.el7.x86_64.rpm
```

```bash
oracleasm createdisk DB1 /dev/sdc1
oracleasm createdisk DB2 /dev/sdd1
oracleasm createdisk DB3 /dev/sde1
oracleasm scandisks
oracleasm listdisks
```



## 安装grid（grid用户）

### 执行检查

```bash
runcluvfy.sh stage -pre crsinst -n ora1,ora2 -fixup -verbose
```

### grid_install.rsp

```ini
oracle.install.responseFileVersion=/oracle/install/rspfmt_crsinstall_response_schema_v11_2_0
# 主机名
ORACLE_HOSTNAME=ora1
INVENTORY_LOCATION=/data/app/grid/oraInventory
SELECTED_LANGUAGES=en
oracle.install.option=CRS_CONFIG
# ORACLE_HOME不能在ORACLE_BASE之下且不相同
ORACLE_BASE=/data/app/grid/base
ORACLE_HOME=/data/app/grid/home
oracle.install.asm.OSDBA=asmdba
oracle.install.asm.OSOPER=asmoper
oracle.install.asm.OSASM=asmadmin
# scan ip
oracle.install.crs.config.gpnp.scanName=ora-scan
oracle.install.crs.config.gpnp.scanPort=1521
# cluster Name
oracle.install.crs.config.clusterName=ora-scan
oracle.install.crs.config.gpnp.configureGNS=false
oracle.install.crs.config.gpnp.gnsSubDomain=
oracle.install.crs.config.gpnp.gnsVIPAddress=
oracle.install.crs.config.autoConfigureClusterNodeVIP=false
# cluster node 前者公网，后者私网
oracle.install.crs.config.clusterNodes=ora1:ora1-vip,ora2:ora2-vip
# network interface list 至少需要2个 ，且填写网关,前者公网，后者私网
oracle.install.crs.config.networkInterfaceList=ens33:192.168.0.0:1,ens37:192.168.245.0:2
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
oracle.install.asm.diskGroup.name=OCRVOTE
oracle.install.asm.diskGroup.redundancy=NORMAL
oracle.install.asm.diskGroup.AUSize=1
# 至少三块
oracle.install.asm.diskGroup.disks=ORCL:DB1,ORCL:DB2,ORCL:DB3
oracle.install.asm.diskGroup.diskDiscoveryString=
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

### 开始静默安装


```bash
./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/grid_install.rsp  -showprogress
```

图形化安装窗口异常的问题解决命令：`./runInstaller -jreLoc /etc/alternatives/jre_1.8.0`

## 关于Centos7的补丁：p18370031

### 或不打补丁

执行root.sh，提示**Adding Clusterware entries to inittab**

```bash
# 编写启动脚本
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

修改服务

```bash
chmod 755 /usr/lib/systemd/system/ohasd.service
systemctl daemon-reload
systemctl enable ohasd.service
```

执行root.sh时，当/etc/init.d.init.ohasd存在时执行`systemctl start ohasd.service`

### 或打补丁

打补丁后执行root.sh，将提示**Adding Clusterware entries to oracle-ohasd.service**

```bash
# 进入补丁目录
cd 18370031/
# 安装补丁
/data/app/grid/home/OPatch/opatch apply
# 检查安装历史记录
/data/app/grid/home/OPatch/opatch lshistory
```

注：如果同时存在 oracle-ohasd.service和ohasd.server，先systemctl disable ohasd.server,然后删除/usr/lib/systemd/system/ohasd.service

## 执行脚本

### 以root用户执行

注：第一个节点执行完俩条命令后，再去二个节点执行

```bash
# 如果不执行perl（root.sh），则/u01/app/11.2.0/grid/bin下
# 没有crsctl、ocrcheck、ocrconfig、ocrdump等，
# 但是有srvctl、crsctl.bin、ocrcheck.bin、ocrconfig.bin、ocrdump.bin
# 命令：/data/app/grid/home/perl/bin/perl -I/data/app/grid/home/perl/lib -I/data/app/grid/home/crs/install /data/app/grid/home/crs/install/rootcrs.pl
/data/app/grid/oraInventory/orainstRoot.sh
/data/app/grid/home/root.sh
```

### 以安装用户grid执行

```bash
/data/app/grid/home/cfgtoollogs/configToolAllCommands RESPONSE_FILE=<response_file>
```

## 检查执行状态

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

# 安装数据库

### 新建oracle用户

```bash
useradd -g oinstall -G oper,dba,asmdba -d /home/oracle oracle
```

### .bash_profile

```bash
su - oracle
vi ~/.bash_profile
export ORACLE_SID=rac
export ORACLE_BASE=/data/app/oracle/base
export ORACLE_HOME=/data/app/oracle/base/home

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
export PATH
```

### 配置节点互信，在主节点用oracle用户执行

```bash
cd database
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user oracle -advanced -noPromptPassphrase
```

### db_install.rsp

```bash
oracle.install.responseFileVersion=/oracle/install/rspfmt_dbinstall_response_schema_v11_2_0
oracle.install.option=INSTALL_DB_SWONLY
ORACLE_HOSTNAME=ora1
UNIX_GROUP_NAME=oinstall
INVENTORY_LOCATION=/data/app/oracle/Inventory
SELECTED_LANGUAGES=en
ORACLE_HOME=/data/app/oracle/base/home
ORACLE_BASE=/data/app/oracle/base
oracle.install.db.InstallEdition=EE
oracle.install.db.EEOptionsSelection=false
oracle.install.db.optionalComponents=oracle.rdbms.partitioning:11.2.0.4.0,oracle.oraolap:11.2.0.4.0,oracle.rdbms.dm:11.2.0.4.0,oracle.rdbms.dv:11.2.0.4.0,o
oracle.install.db.DBA_GROUP=dba
oracle.install.db.OPER_GROUP=dba
oracle.install.db.CLUSTER_NODES=ora1,ora2
oracle.install.db.isRACOneInstall=false
oracle.install.db.racOneServiceName=
oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
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

### 进行安装

注：然后我在第二个节点执行以下语句， 因为第一个节点报错GI未安装

注：每个节点需要有相同文件结构

```bash
./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/db_install.rsp -showProgress
```

