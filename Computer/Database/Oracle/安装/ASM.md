# 通过asmlib创建asm磁盘组

## 安装依赖

```bash
$ wget https://yum.oracle.com/repo/OracleLinux/OL7/latest/x86_64/getPackage/oracleasm-support-2.1.11-2.el7.x86_64.rpm
$ wget https://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el7.x86_64.rpm
$ yum install kmod-oracleasm
$ rpm -ivh oracleasm-support-2.1.11-2.el7.x86_64.rpm
$ rpm -ivh oracleasmlib-2.0.12-1.el7.x86_64.rpm
```



## 基本配置

注：以root用户启动oracleasm，且后续无需以设定的用户运行

```bash
root@ora1 grid $ oracleasm configure -i
Configuring the Oracle ASM library driver.

This will configure the on-boot properties of the Oracle ASM library
driver.  The following questions will determine whether the driver is
loaded on boot and what permissions it will have.  The current values
will be shown in brackets ('[]').  Hitting <ENTER> without typing an
answer will keep that current value.  Ctrl-C will abort.

Default user to own the driver interface []: grid
Default group to own the driver interface []: oinstall
Start Oracle ASM library driver on boot (y/n) [n]: y
Scan for Oracle ASM disks on boot (y/n) [y]: y
Writing Oracle ASM library driver configuration: done
```

## 初始化

```bash
root@ora1 grid $ oracleasm init
Creating /dev/oracleasm mount point: /dev/oracleasm
Loading module "oracleasm": oracleasm
Configuring "oracleasm" to use device physical block size
Mounting ASMlib driver filesystem: /dev/oracleasm
```

## 查看状态

```bash
root@ora1 grid $ oracleasm status
Checking if ASM is loaded: yes
Checking if /dev/oracleasm is mounted: yes
```

## 增加asm(至少需要三个)

```bash
root@ora1 ~ $ oracleasm createdisk db1 /dev/sdb1
Writing disk header: done
Instantiating disk: done
root@ora1 ~ $ oracleasm createdisk db2 /dev/sdb2
Writing disk header: done
Instantiating disk: done
```

## 检查asm

```bash 
root@ora1 ~ $ oracleasm listdisks
DB1
DB2
```

