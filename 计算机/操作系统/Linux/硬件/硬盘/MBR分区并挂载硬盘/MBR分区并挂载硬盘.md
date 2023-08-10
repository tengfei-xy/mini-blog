# MBR分区并挂载硬盘

## 目录

-   [查看操作对象](#查看操作对象)
-   [进行分区](#进行分区)
-   [格式化分区](#格式化分区)
-   [挂载分区](#挂载分区)
    -   [临时挂载方法](#临时挂载方法)
    -   [永久挂载方法](#永久挂载方法)
-   [测试分区](#测试分区)

## 查看操作对象

```bash
root@ora1 ~ $ fdisk -l

磁盘 /dev/sdb：5368 MB, 5368709120 字节，10485760 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节


磁盘 /dev/sda：53.7 GB, 53687091200 字节，104857600 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x000b1a75

   设备 Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   104857599    51379200   8e  Linux LVM

磁盘 /dev/mapper/centos-root：48.4 GB, 48444211200 字节，94617600 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节


磁盘 /dev/mapper/centos-swap：4160 MB, 4160749568 字节，8126464 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
```

## 进行分区

```bash
root@ora1 ~ $ fdisk /dev/sdb
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

Device does not contain a recognized partition table
使用磁盘标识符 0xd787b429 创建新的 DOS 磁盘标签。

命令(输入 m 获取帮助)：n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): e
分区号 (1-4，默认 1)：1
起始 扇区 (2048-10485759，默认为 2048)：
将使用默认值 2048
Last 扇区, +扇区 or +size{K,M,G} (2048-10485759，默认为 10485759)：
将使用默认值 10485759
分区 1 已设置为 Extended 类型，大小设为 5 GiB

命令(输入 m 获取帮助)：w
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。
```

## 格式化分区

```bash
root@ora1 ~ $ mkfs.xfs -f /dev/sdb
```

## 挂载分区

```bash
root@ora1 ~ $ mkdir /data/app/oracle/
```

### 临时挂载方法

```bash
root@ora1 ~ $ mount -t xfs /dev/sdb /data/app/oracle/
```

### 永久挂载方法

```bash
root@ora1 ~ $ echo "/dev/sdb    /data/app/oracle/    xfs    defaults    0 0" >> /etc/fstab
```

## 测试分区

```bash
root@ora1 ~ $ df -Th /data/app/oracle/racdata
文件系统       类型  容量  已用  可用 已用% 挂载点
/dev/sdb       xfs   5.0G   33M  5.0G    1% /data/app/oracle/racdata
```
