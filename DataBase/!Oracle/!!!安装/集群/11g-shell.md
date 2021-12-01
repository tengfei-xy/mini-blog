```bash
dt=oracle
op=install
ht=rac              # type: single/rac/ha
v=11G               # version: 11G/19C/21C
i=192.168.7.2       # Public ip
n=ora               # hostname: will aotu add "db" for single/ha,add "db1/db2"for  rac
rp=bb123456         # root password
dp=oracle           # db password: os: oracle/grid db: SYS/SYSTEM/SYSMAN/DBSNMP 
o=orcl              # oraclesid
cdb=false           # createAsContainerDatabase: true/false
pdbname=pdb         # pdbname
cs=ZHS16GBK         # characterset: ZHS16GBK/AL32UTF8 
ncs=AL16UTF16       # NCHARACTERSET: AL16UTF16/UTF8 
yum=/dev/cdrom      # yum location: /dev/cdrom or /soft/yum.iso
pu1=192.168.7.2     # public ip
pu2=192.168.7.3     # public ip
vip1=192.168.7.4     # virtual ip
vi2=192.168.7.5     # virtual ip
pr1=192.168.207.2   # private ip
pr2=192.168.207.3   # private ip
si=192.168.7.6      # scan ip
puf=ens33           # 主机的公网IP对应的网卡名称
prf=ens34           # 主机的私有IP对应的网卡名称
or=NORMAL           # asm ocr redundancy: high/normal/external
od=/dev/sdc,/dev/sde,/dev/sdf        # asm ocr disk
dr=EXTERNAL         # asm data redundancy: high/normal/external
dd=/dev/sdd         # asm data di

sh x_onekey.sh -dt="$dt" -op="$op" -ht="$ht" -v="$v" -i="$i" -n="$n" -rp="$rp" -dp="$dp" -o="$o" -cdb="$cdb" -pdbname="$pdbname" -cs="$cs" -ncs="$ncs" -yum="$yum" -pu1="$pu1" -pu2="$pu2" -vi1="$vi1" -vi2="$vi2" -pr1="$pr1" -pr2="$pr2" -si="$si" -puf="$puf" -prf="$prf" -or="$or" -od="$od" -dr="$dr" -dd="$dd"

```

