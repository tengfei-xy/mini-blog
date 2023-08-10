# mpp安装

创建用户

```纯文本
groupadd gbase
useradd -g gbase -d /home/gbase -s /bin/bash -m gbase
echo gbase | passwd gbase

```

解压安装包

```纯文本
tar -xjf GBase8a_MPP_Cluster-License-9.5.3.14-redhat7.3-x86_64.tar.bz2
```

准备目录

```纯文本
mkdir /opt/8a && chown gbase:gbase /opt/8a
```

6、 安装环境准备：root用户执行 cd gcinstall;python SetSysEnv.py --dbaUser=gbase --installPrefix=/opt/8a

7、 切换到集群安装及运行用户，并修改安装配置文件参数

\#su - gbase

\$vi /opt/gcinstall/demo.options

```纯文本
installPrefix= /opt/8a （安装目录）
coordinateHost = 172.16.100.158 （Coordinator节点IP地址）
coordinateHostNodeID = 158 （Coordinator节点Nodeid）
dataHost = 172.16.100.158 (Data节点IP地址）
#existCoordinateHost = 
#existDataHost = 
gcwareHost = 172.16.100.158（gcware节点IP地址）
gcwareHostNodeID = 158（gcware节点nodeid）
dbaUser = gbase 
dbaGroup = gbase 
dbaPwd = 'gbase' （gbase用户密码）
rootPwd = '111111' （root用户密码）
#rootPwdFile = rootPwd.json
```

8、 执行安装 gbase用户，进入gcinstall目录

./gcinstall.py --silent=demo.options

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wps72IFIG.jpg)

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpssDLL5b.jpg)

9、 采集指纹

```纯文本
./gethostsid -n 172.16.100.158 -u root -p GBASEtest890 -f /home/gbase/lic.txt
```

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsFUKxUc.jpg)

下载/home/gbase/lic.txt发送给技术支持获取license文件

10、 加载license，gbase用户，gcinstall目录

```纯文本
./License -n 172.16.100.158 -u gbase -p Gbase@123 -f /home/gbase/158.lic
```

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsUosbGS.jpg)

11、 查看license状态

```纯文本
./chkLicense -n 172.16.100.158 -u gbase -p Gbase@123
```

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsj0gI76.jpg)

12、 启动服务

加载环境变量source /home/gbase/.bash\_profile

启动gcware进程：gcware\_services all restart

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wps1kNejy.jpg)

启动gcluster进程：gcluster\_services all restart

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsXg8LnJ.jpg)

查询进程状态：gcadmin

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsa8q4z8.jpg)

13、 初始化配置

***创建******创建******​******​*** ***distribution******distribution******​******​***\*\* \*：******：******：******：******​\*\* ​\*\* ​\*\* ​\*\*\*

***\$cat /opt/gcinstall/gcChangeInfo.xml***\*\* \*\$cat /opt/gcinstall/gcChangeInfo.xml******​******​\*\*\*

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsxW9ook.jpg)

***gcadmin distribution gcChangeInfo.xml p 1 d 0******gcadmin distribution gcChangeInfo.xml p 1 d 0******​******​***

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsl8jaiv.jpg)

初始化：***gccli -uroot******gccli -uroot******​******​***（root 用户默认密码是空；gbase 用户密码是

“gbase20110531”），执行 initnodedatamap 命令

![](file:////private/var/folders/9b/t1nnnj9d1sx8ckkvtf8722j80000gn/T/com.kingsoft.wpsoffice.mac/wps-tengfei/ksohtml/wpsBp47dB.jpg)
