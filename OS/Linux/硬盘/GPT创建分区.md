获取分区类型：

```
fdisk -l
```

针对`磁盘标签类型：gpt`使用parted命令：

```bash
[root@localhost ~]# parted /dev/sda
GNU Parted 3.1
使用 /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.

(parted) mkpart                                                           
分区名称？  []? kernel                                                    
文件系统类型？  [ext2]? xfs                                               
起始点？ 21.5GB                                                           
结束点？ 80GB

(parted) print                                                            
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sda: 85.9GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 
Number  Start   End     Size    File system  Name                  标志
 1      1049kB  211MB   210MB   fat16        EFI System Partition  启动
 2      211MB   1285MB  1074MB  xfs
 3      1285MB  21.5GB  20.2GB                                     lvm
 4      21.5GB  80.0GB  58.5GB               kernel


```

