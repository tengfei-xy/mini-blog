

# 一、 系统配置

1. 安装依赖

   - 12.2.0+RHEL7

     本地系统镜像中包含的

     ```
     binutils.x86_64 compat-libcap1.x86_64 gcc.x86_64 gcc-c++.x86_64 glibc.x86_64 glibc-devel.x86_64 libaio.x86_64 libaio-devel.x86_64 libgcc.i686 libgcc.x86_64 libstdc++.x86_6 libstdc++-devel.x86_64 libXi.x86_64 libXtst.x86_64 make.x86_64 sysstat.x86_64 compat-libstdc++-33 elfutils-libelf-devel net-tools ksh smartmontools unzip
     ```

     额外需要

     ```bash
     compat-libstdc++-33-3.2.3-72.el7.x86_64.rpm
     ```

     

2. 用户配置

   ```shell
   groupadd oinstall
   groupadd oper
   groupadd dba
   groupadd backupdba
   groupadd dgdba
   groupadd kmdba
   groupadd racdba
   useradd -g oinstall -G oper,dba,backupdba,dgdba,kmdba,racdba -d /home/oracle oracle
   echo "oracle" | passwd oracle --stdin
   
   ```

   编辑~/.bash_profile

   ```shell
   export ORACLE_BASE=/u01/app/oracle
   export ORACLE_HOME=/u01/app/oracle/product/12.2.0/db_1
   export ORACLE_SID=oracle
   
   # sqlplus 使用的一些.so文件
   export LD_LIBRARY_PATH=/u01/app/oracle/product/12.2.0/db_1/stage/ext/lib
   PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin
   export PATH
   ```

   生效文件

   ```
   . ~/.bash_profile
   ```


3. 内核参数

   ```shell
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

   用户资源：修改/etc/security/limits.conf

   ```shell
   oracle   soft   nproc    131072
   oracle   hard   nproc    131072
   oracle   soft   nofile   131072
   oracle   hard   nofile   131072 
   oracle   soft   stack    10240
   oracle   hard   stack    32768
   oracle   soft   memlock  50000000
   oracle   hard   memlock  50000000
   ```

   在/etc/pam.d/login下添加

   ```
   session required pam_limits.so
   ```

   这使得用户将加载PAM的pam_limits.so，设置用户登录的硬性设置

   

## 二、oracle安装

1. 文件夹准备

   ```bash
   mkdir -p /u01/app/oracle/product/12.2.0/db_1
   chown -R oracle:oinstall /u01
   ```

2. 安装：进入database目录，用oracle用户运行

   ```bash
   ./runInstaller -silent -force \
   oracle.install.option=INSTALL_DB_SWONLY \
   UNIX_GROUP_NAME=oinstall \
   INVENTORY_LOCATION=/u01/app/oraInventory \
   ORACLE_HOME=/u01/app/oracle/product/12.2.0/db_1 \
   ORACLE_BASE=/u01/app/oracle \
   oracle.install.db.InstallEdition=EE \
   oracle.install.db.isCustomInstall=false \
   oracle.install.db.DBA_GROUP=dba \
   oracle.install.db.OPER_GROUP=oper \
   oracle.install.db.OSBACKUPDBA_GROUP=backupdba \
   oracle.install.db.OSDGDBA_GROUP=dgdba \
   oracle.install.db.OSKMDBA_GROUP=kmdba \
   oracle.install.db.OSRACDBA_GROUP=racdba \
   DECLINE_SECURITY_UPDATES=true
   ```

3. root.sh：运行root的俩个脚本

   ```bash
   /u01/app/oraInventory/orainstRoot.sh
   /u01/app/oracle/product/12.2.0/db_1/root.sh
   ```

4. 创建并启动监听

   ```bash
   cd $ORACLE_HOME
   netca -silent -responseFile `pwd`/assistants/netca/netca.rsp
   ```

5. 查看状态

   ```bash
   lsnrctl status
   ```

   

## 三、初始化实例

- 方法一：使用资源文件

  1. 编辑assistants/dbca/dbca.rsp

     ```bash
     gdbName=
     SID=
     templateName=tempGeneral_Purpose.dbc
     sysPassword=
     SYSTEMPASSWORD=
     CHARACTERSET=""
     ```

  2. 初始化数据库

     ```bash
     dbca -createdatabase -silent -responseFile `pwd`/assistants/dbca/dbca.rsp
     ```

- 方法二：使用命令参数

  详细配置

  ```bash
  dbca -silent -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb3 -sid cdb3 -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb3 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -memoryMgmtType auto_sga \
   -totalMemory 1536 \
   -storageType FS \
   -datafileDestination "/u01/app/oracle/oradata/" \
   -redoLogFileSize 50 \
   -emConfiguration NONE \
   -ignorePreReqs
  ```

  简单配置

  ```bash
  dbca -silent -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname orcl_dst -sid orcl -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword Sino_ZJ001 \
   -systemPassword Sino_ZJ001 \
   -emConfiguration NONE \
   -ignorePreReqs
  ```

  

