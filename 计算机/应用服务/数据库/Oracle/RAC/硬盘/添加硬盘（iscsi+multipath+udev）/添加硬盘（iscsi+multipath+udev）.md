# 添加硬盘（iscsi+multipath+udev）

## 目录

-   [添加硬盘iscsi+multipath+udev方式](#添加硬盘iscsimultipathudev方式)
    -   [iscsi设置](#iscsi设置)
    -   [multipath设置(每个节点执行)](#multipath设置每个节点执行)
    -   [配置udev（双节点执行）](#配置udev双节点执行)
    -   [sqlplus添加硬盘](#sqlplus添加硬盘)

# 添加硬盘iscsi+multipath+udev方式

## iscsi设置

注：在targetcli中执行正确均有绿色字回显，下文删除了输出日志。

1.  确定需要加到rac的硬盘，在例中使用`/dev/sdc`
    ```bash
    [root@ora1 crsd]# lsblk
    NAME             MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sda                8:0    0   50G  0 disk  
    ├─sda1             8:1    0    1G  0 part  /boot
    └─sda2             8:2    0   49G  0 part  
      ├─centos-root  253:0    0 45.1G  0 lvm   /
      └─centos-swap  253:1    0  3.9G  0 lvm   [SWAP]
    sdb                8:16   0   20G  0 disk  
    ├─sdb1             8:17   0    6G  0 part  
    ├─sdb2             8:18   0    6G  0 part  
    └─sdb3             8:19   0    8G  0 part  
    sdc                8:32   0    5G  0 disk  
    sdd                8:48   0    6G  0 disk  
    └─RAC-OCRVOTE    253:2    0    6G  0 mpath 
      └─RAC-OCRVOTE1 253:6    0    6G  0 part  
    sde                8:64   0    8G  0 disk  
    └─RAC-FRA        253:4    0    8G  0 mpath 
      └─RAC-FRA1     253:7    0    8G  0 part  
    sdf                8:80   0    6G  0 disk  
    └─RAC-DATA       253:3    0    6G  0 mpath 
      └─RAC-DATA1    253:5    0    6G  0 part  
    sr0               11:0    1  4.5G  0 rom 
    ```
2.  对/dev/sdc分区
    ```bash
    [root@ora1 crsd]# fdisk /dev/sdc
    命令(输入 m 获取帮助)：n
    Select (default p): p
    分区号 (1-4，默认 1)：1
    起始 扇区 (2048-10485759，默认为 2048)：2048
    Last 扇区, +扇区 or +size{K,M,G} (2048-10485759，默认为 10485759)：10485759
    命令(输入 m 获取帮助)：w
    正在同步磁盘。
    ```
3.  将目标磁盘添加到iscsi服务端
    可以看到，`/backstores/block/`下有DB1、DB2、DB3
    ```纯文本
    [root@ora1 crsd]# targetcli 
    /> ls /backstores/block
    o- block .................................................. [Storage Objects: 3]
      o- DB1 ............................. [/dev/sdb1 (6.0GiB) write-thru activated]
      | o- alua ................................................... [ALUA Groups: 1]
      |   o- default_tg_pt_gp ....................... [ALUA state: Active/optimized]
      o- DB2 ............................. [/dev/sdb2 (6.0GiB) write-thru activated]
      | o- alua ................................................... [ALUA Groups: 1]
      |   o- default_tg_pt_gp ....................... [ALUA state: Active/optimized]
      o- DB3 ............................. [/dev/sdb3 (8.0GiB) write-thru activated]
        o- alua ................................................... [ALUA Groups: 1]
          o- default_tg_pt_gp ....................... [ALUA state: Active/optimized]
    ```
    创建新块,/dev/sdc1是第二步分区的结果
    ```纯文本
    />  /backstores/block create DB4 /dev/sdc1
    Created block storage object DB4 using /dev/sdc1.
    ```
4.  查看iscsi使用的iqn，在下图中使用的是iqn.2021-07.02.com.oracle（即行3）
    ```纯文本
    /> ls /iscsi/
    o- iscsi .......................................................... [Targets: 1]
      o- iqn.2021-07.02.com.oracle ....................................... [TPGs: 1]
        o- tpg1 ............................................. [no-gen-acls, no-auth]
          o- acls ........................................................ [ACLs: 1]
          | o- iqn.2021-07.02.com.oracle:ora1 ..................... [Mapped LUNs: 3]
          |   o- mapped_lun0 ................................. [lun0 block/DB1 (rw)]
          |   o- mapped_lun1 ................................. [lun1 block/DB2 (rw)]
          |   o- mapped_lun2 ................................. [lun2 block/DB3 (rw)]
          o- luns ........................................................ [LUNs: 3]
          | o- lun0 ..................... [block/DB1 (/dev/sdb1) (default_tg_pt_gp)]
          | o- lun1 ..................... [block/DB2 (/dev/sdb2) (default_tg_pt_gp)]
          | o- lun2 ..................... [block/DB3 (/dev/sdb3) (default_tg_pt_gp)]
          o- portals .................................................. [Portals: 1]
            o- 0.0.0.0:3260 ................................................... [OK]
    /> cd /iscsi/iqn.2021-07.02.com.oracle/tpg1/luns/
    /iscsi/iqn.20...cle/tpg1/luns>create /backstores/block/DB4
    ```
5.  退出并重启服务
    ```纯文本
    /iscsi/iqn.20...02.com.oracle> exit
    [root@ora1 crsd]#  systemctl restart target
    ```
    注：此时查看应登录iscsi的rac节点上，应有看到新增的共享盘

## multipath设置(每个节点执行)

1.  添加共享硬盘到wwids
    ```bash
    [root@ora1 etc]# multipath -a /dev/sdg
    wwid '36001405181030e174b74aceb74af1647' added
    ```
2.  编辑multipath.conf，将新硬盘的wwid添加到多路径软件下
    ```纯文本
    [root@ora1 etc]# cat /etc/multipath.conf
    defaults {
            user_friendly_names yes
            path_grouping_policy multibus 
            find_multipaths yes
            path_selector "round-robin 0"
            failback manual
            rr_weight priorities
            no_path_retry 5
    }

    blacklist {
           devnode "^sd[ab]" 
           devnode "sr0"
    }
    multipaths {
           multipath {
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
           # 下方内容为新增内容
           multipath {
                  wwid 36001405181030e174b74aceb74af1647
                  alias RAC-DATA2
    	     }
    }
    ```
3.  重启服务
    ```纯文本
    [root@ora1 etc]# systemctl restart multipathd
    ```
4.  查看新增的硬盘，可以看到`/dev/sdg`下已有`RAC-DATA2`
    ```bash
    [root@ora1 etc]# lsblk
    NAME             MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sda                8:0    0   50G  0 disk  
    ├─sda1             8:1    0    1G  0 part  /boot
    └─sda2             8:2    0   49G  0 part  
      ├─centos-root  253:0    0 45.1G  0 lvm   /
      └─centos-swap  253:1    0  3.9G  0 lvm   [SWAP]
    sdb                8:16   0   20G  0 disk  
    ├─sdb1             8:17   0    6G  0 part  
    ├─sdb2             8:18   0    6G  0 part  
    └─sdb3             8:19   0    8G  0 part  
    sdc                8:32   0    5G  0 disk  
    └─sdc1             8:33   0    5G  0 part  
    sdd                8:48   0    6G  0 disk  
    └─RAC-OCRVOTE    253:2    0    6G  0 mpath 
      └─RAC-OCRVOTE1 253:6    0    6G  0 part  
    sde                8:64   0    8G  0 disk  
    └─RAC-FRA        253:4    0    8G  0 mpath 
      └─RAC-FRA1     253:7    0    8G  0 part  
    sdf                8:80   0    6G  0 disk  
    └─RAC-DATA       253:3    0    6G  0 mpath 
      └─RAC-DATA1    253:5    0    6G  0 part  
    sdg                8:96   0    5G  0 disk  
    └─RAC-DATA2      253:8    0    5G  0 mpath 
    sr0               11:0    1  4.5G  0 rom
    ```

## 配置udev（双节点执行）

1.  查询DM\_UUID
    ```bash
    [root@ora1 etc]#udevadm info --query=all --name=/dev/mapper/RAC-DATA2  |grep -i -o dm_uuid.*
    DM_UUID=mpath-36001405181030e174b74aceb74af1647
    ```
2.  编辑udev规则，在/etc/udev/rules.d目录下根据原来rules文件,添加一下内容
    ```纯文本
    KERNEL=="dm-*",ENV{DM_UUID}=="mpath-36001405181030e174b74aceb74af1647",OWNER="grid",GROUP="asmadmin",MODE="0660"
    ```
3.  重启udev服务
    ```纯文本
    udevadm control --reload-rules
    udevadm trigger --type=devices --action=change
    ```

## sqlplus添加硬盘

1.  以grid身份运行
    ```纯文本
    sqlplus / as sysasm
    ```
2.  查看当前硬盘组容量
    ```纯文本
    SQL> select name,state,total_mb,free_mb from v$asm_diskgroup where name='DATA';
    NAME			       STATE	     TOTAL_MB	 FREE_MB
    ------------------------------ ----------- ---------- ----------
    DATA			       MOUNTED		 6140	    4476
    ```
3.  添加硬盘
    ```纯文本
     sql> ALTER DISKGROUP data ADD DISK '/dev/dm-8';
     Diskgroup altered.
    ```
4.  查看
    -   当前空余空间
    ```纯文本
    SQL> select name,state,total_mb,free_mb from v$asm_diskgroup where name='DATA';

    NAME			       STATE	     TOTAL_MB	 FREE_MB
    ------------------------------ ----------- ---------- ----------
    DATA			       MOUNTED		11259	    9593
    ```
    -   查看磁盘组的文件
    ```纯文本
    SQL> select name,PATH from v$asm_disk;

    NAME             PATH
    ---------------- --------------------
    FRA_0000         /dev/dm-7
    OCRVOTE_0000     /dev/dm-6
    DATA_0000        /dev/dm-5
    DATA_0001        /dev/dm-8
    ```
