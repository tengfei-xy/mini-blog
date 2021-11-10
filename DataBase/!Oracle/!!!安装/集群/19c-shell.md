[GitHub-InstallOracleShell](https://github.com/pc-study/InstallOracleshell)

[README](https://github.com/pc-study/InstallOracleshell/blob/main/ParameterREADME.md)

```bash
i=192.168.119.131 # node1 Public ip
n=ora             # rac hostname
c=true            # 判断是否为CDB模式
pb=rac            # 创建PDB的名称
o=rac 	          # oraclesid
rs=tengfei        # root password
op=oracle         # oracle password
gp=grid           # grid password
b=/u01/app			  # install basedir
s=AL32UTF8			  # characterset
ns=UTF8           # national characterset
pb1=$i            # node public ip
pb2=192.168.119.132 # node public ip
vi1=192.168.119.133 # node virtual ip
vi2=192.168.119.134 # node virtual ip
pi1=192.168.207.133 # node private ip
pi2=192.168.207.136 # node private ip
si=192.168.119.135  # scan ip
od="/dev/sdc,/dev/sde,/dev/sdf" # asm ocr disk
dd="/dev/sdd"     # asm data disk
on=OCR            # asm ocr diskgroupname
dn=DATA           # asm data diskgroupname
or=NORMAL         # asm ocr redundancy
dr=EXTERNAL       # asm data redundancy
puf=ens33         # 主机的访问IP对应的网卡名称
prf=ens36         # 主机的私有IP对应的网卡名称
./OracleShellInstall.sh -i $i -n $n -c $c -pb $pb -o $o -rs $rs -op $op -gp $gp -b $b -s $s -ns $ns -pb1 $pb1 -pb2 $pb2 -vi1 $vi1 -vi2 $vi2 -pi1 $pi1 -pi2 $pi2 -si $si -od $od -dd $dd -on $on -dn $dn -or $or -dr $dr -puf $puf -prf $prf

```

> 1. 安装脚本的multipath.conf默认devonode /dev/sda（1602行左右），建议去除，因为当根目录使用/dev/vda时，iscsi的共享磁盘就会从sda开始