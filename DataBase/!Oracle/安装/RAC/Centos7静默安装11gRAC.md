操作系统：Centos7.8

oracle：11g 11.2.0.4

# 安装grid

## 准备工作

关闭防火墙和SELinux

```bash
systemctl disable firewalld
# 永久关闭SELINUX
sed -i 's/=enforcing/=disabled/g'  /etc/selinux/config
reboot
```

修改内核参数/etc/sysctl.conf

```bash
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
```

生效内核参数

```
sysctl -p
```

修改/etc/security/limits.conf 

```bash
grid soft nproc 2047
grid hard nproc 16384
grid soft nofile 1024
grid hard nofile 65536
```

### 安装依赖

外部依赖包

```bash
yum install -y binutils-* libc* compat-libstdc++-* elfutils-libelf-* elfutils-libelf-* elfutils-libelf-devel-static-* gcc-* gcc-c++-* glibc-* glibc-common-* glibc-devel-* glibc-headers-* kernel-headers-* ksh-* libaio-* libaio-devel-* libgcc-* libgomp-* libstdc++-* libstdc++-devel-* make-* sysstat-* compat-libcap* smartmontools
```

 安装cvuqdisk包

```bash
# 进入安装文件夹
cd grid/
rpm -ivh rpm/cvuqdisk-1.0.9-1.rpm
```

### gird 用户设置

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

 .bash_profile

```shell
export ORACLE_BASE=/u01/app/grid/base
export ORACLE_HOME=/u01/app/grid/home
export ORACLE_SID=+ASM1
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
export PATH
```

生效环境变量

```
. ~/.bash_profile
```

配置RAC的节点互信，在主节点用grid用户执行

```bash
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user grid -advanced -noPromptPassphrase
```

注：验证互信的方式：ssh 主机名 date，能免密执行

## 安装共享磁盘（root用户执行）

### 安装iscsi服务端

### 安装iscsi客户端

### 安装ASMLIB驱动与配置

```bash
fdisk /dev/sdc
fdisk /dev/sdd
fdisk /dev/sde
chown -R grid:oinstall /dev/sdc1
chown -R grid:oinstall /dev/sdd1
chown -R grid:oinstall /dev/sde1
```

安装asmlib驱动

```bash
rpm -ivh kernel-3.10.0-1160.11.1.el7.x86_64.rpm kernel-3.10.0-1160.15.2.el7.x86_64.rpm
rpm -ivh kmod-oracleasm-2.0.8-28.el7.x86_64.rpm oracleasmlib-2.0.12-1.el7.x86_64.rpm oracleasm-support-2.1.11-2.el7.x86_64.rpm
```

初始化磁盘

```
oracleasm configure -i
oracleasm init
```

创建磁盘

```bash
oracleasm createdisk DB1 /dev/sdc1
oracleasm createdisk DB2 /dev/sdd1
oracleasm createdisk DB3 /dev/sde1
```

扫描磁盘

```
oracleasm scandisks
```

查看磁盘

```
oracleasm listdisks
```

## 安装grid（grid用户执行）

执行检查

```bash
./runcluvfy.sh stage -pre crsinst -n ora1,ora2 -fixup -verbose
```

grid_install.rsp

```ini
oracle.install.responseFileVersion=/oracle/install/rspfmt_crsinstall_response_schema_v11_2_0
# 主机名
ORACLE_HOSTNAME=ora1
# INVENTORY 目录
INVENTORY_LOCATION=/u01/app/grid/oraInventory
SELECTED_LANGUAGES=en
oracle.install.option=CRS_CONFIG
# ORACLE_HOME不能在ORACLE_BASE之下且不相同
ORACLE_BASE=/u01/app/grid/base
ORACLE_HOME=/u01/app/grid/home
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
# 且填写网关,前者公网，后者私网，填写该IP的网段号
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
# 表决盘
oracle.install.asm.diskGroup.name=OCRVOTE
# NORMAL模式需要指定三块，EXTERNAL模式只需要制定一块盘
# 剩下2块盘，留作创建磁盘组
oracle.install.asm.diskGroup.redundancy=EXTERNAL
oracle.install.asm.diskGroup.AUSize=1
# 指定asm磁盘路径
oracle.install.asm.diskGroup.disks=/dev/oracleasm/disks/DB1
# 发现其他asm磁盘
oracle.install.asm.diskGroup.diskDiscoveryString=/dev/oracleasm/disks/*
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

开始静默安装（约十分钟）


```bash
./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/grid_install.rsp  -showprogress
```

注：图形化安装窗口异常的问题解决命令：`./runInstaller -jreLoc /etc/alternatives/jre_1.8.0`

注：通过`echo $?`是否返回`0`来判断安装成功

## 关于Centos7服务的补丁：p18370031

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

在运行root.sh时，`/etc/init.d/`目录出现`inint.ohasd`时启动服务

```
systemctl start ohasd.service
```

### 或打补丁

打补丁后执行root.sh，将提示**Adding Clusterware entries to oracle-ohasd.service**

```bash
# 进入补丁目录
cd 18370031/
# 安装补丁
/u01/app/grid/home/OPatch/opatch apply
# 检查安装历史记录
/u01/app/grid/home/OPatch/opatch lshistory
```

## 执行脚本

### 以root用户执行

注：第一个节点执行完俩条命令后，再去二个节点执行

注，多检查日志，执行完毕后可`ehco $?`查看返回值，0则正确

```bash
/u01/app/grid/oraInventory/orainstRoot.sh
# 约十分钟
/u01/app/grid/home/root.sh
```

### 在主节点以grid用户执行

```bash
/u01/app/grid/home/cfgtoollogs/configToolAllCommands RESPONSE_FILE=<response_file>
```

注：plug-in Automatic Storage Management Configuration Assistan失败关系不大($?返回3)

### 结束：检查RAC状态

注：gsd结尾项状态为OFFLINE是正常

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

## 用户配置

新建oracle用户

```bash
useradd -g oinstall -G oper,dba,asmdba -d /home/oracle oracle
```

.bash_profile

```bash
su - oracle
vi ~/.bash_profile
export ORACLE_SID=rac
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/db_1

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
export PATH
```

 配置RAC的节点互信，在主节点用oracle用户执行

```bash
# 进入安装目录
cd database
sshsetup/sshUserSetup.sh -hosts "ora1 ora2" -user oracle -advanced -noPromptPassphrase
```

注：互信检查方式为：``ssh 主机名 date`,能免密执行

## 安装数据库

db_install.rsp

```bash
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

### 主节点执行进行安装

注：每个节点需要有相同文件结构

注：其他远程节点会自动复制，远程节点的$ORACLE_HOME大约有4.2G，可查看进度（大约十五分钟）
```bash
./runInstaller -ignorePrereq -silent -force -responseFile `pwd`/response/db_install.rsp -showProgress
```

### 执行脚本

注：执行用户：root用户

注：主节点执行后，去其他节点执行

```
/u01/app/oracle/product/11.2.0/db_1/root.sh
```

## 创建磁盘组（在节点一上执行）

以grid用户进入SQLPLUS

```
 sqlplus / as sysasm
```

查看当前磁盘组

```sql
 SQL> select group_number,name,state,total_mb,free_mb from v$asm_diskgroup;
 GROUP_NUMBER NAME			    STATE	  TOTAL_MB    FREE_MB
------------ ------------------------------ ----------- ---------- ----------
	   1 OCRVOTE			    MOUNTED	      1456	 1024
```

创建磁盘组

```
 SQL> CREATE DISKGROUP DATA external REDUNDANCY disk '/dev/oracleasm/disks/DB2' ATTRIBUTE 'au_size'='1M','compatible.asm'='11.2';
Diskgroup created.

 SQL> CREATE DISKGROUP FRA external REDUNDANCY disk '/dev/oracleasm/disks/DB3' ATTRIBUTE 'au_size'='1M','compatible.asm'='11.2';
 Diskgroup created.
```

再次查看当前磁盘组

```
SQL> select group_number,name,state,total_mb,free_mb from v$asm_diskgroup;

GROUP_NUMBER NAME			    STATE	  TOTAL_MB    FREE_MB
------------ ------------------------------ ----------- ---------- ----------
	   1 OCRVOTE			    MOUNTED	      1456	 1024
	   2 DATA			    MOUNTED	      2186	 2134
	   3 FRA			    MOUNTED	      1460	 1408
```

## 静默建库

dbca.rsp，主要修改CREATEDATABASE节的以下内容

```ini
[CREATEDATABASE]
GDBNAME = "racdb"
SID = "rac"
NODELIST=rac1,rac2
TEMPLATENAME = "General_Purpose.dbc"
SYSPASSWORD = "password"
SYSTEMPASSWORD = "password"
DATAFILEDESTINATION ="+DATA"
RECOVERYAREADESTINATION="+DATA"
STORAGETYPE=ASM
CHARACTERSET = "AL32UTF"
NATIONALCHARACTERSET= "UTF8"
TOTALMEMORY = "800"
```

以oracle用户执行建库

注：如果运行时直接结束没有任何输出，建议从图形化方式执行rbca，日志位于`$ORACLE_HOME/cfgtoollogs/dbca`

```
cd $ORACLE_HOME
dbca -silent -responseFile `pwd`/assistants/dbca/dbca.rsp
```

### 节点一修改archivelog

```
SQL> startup mount
SQL> alter system set log_archive_dest_1='location=+ARCH';
```

此时另一节点查看log_archive_dest_1,结果如下

```sql
 SQL> show parameter log_archive_dest_1
 NAME				     TYPE	 VALUE
------------------------------------ ----------- ------------------------------
log_archive_dest_1		     string	 location=+ARCH
log_archive_dest_10		     string
log_archive_dest_11		     string
log_archive_dest_12		     string
log_archive_dest_13		     string
log_archive_dest_14		     string
log_archive_dest_15		     string
log_archive_dest_16		     string
log_archive_dest_17		     string
log_archive_dest_18		     string
log_archive_dest_19		     string
```

# 重启测试

## 数据库重启

双节点同时执行

```
shutdown immediate;
```

第一节点，启动数据库到 mount 状态，开启归档模式，最后正常打开数据库 

注意：节点处于Archive Mode时不能startup

```
SQL> startup mount;
SQL> archive log list;
SQL> alter database archivelog;
SQL> archive log list;
```

第二节点直接启动

```
SQL> startup;
```

## 集群重启

第一节点用root用户直接关闭并启动集群

注：启动/关闭集群需要root权限，可以吧grid用户变量复制到root用户的.bash_profile下

```
# 关得快
[root@ora1 ~]# crsctl stop cluster -all
# 启动慢
[root@ora1 ~]# crsctl start cluster -all
```

