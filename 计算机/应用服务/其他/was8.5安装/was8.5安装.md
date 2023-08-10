# was8.5安装

注：本文只安装AppServer，不安装IBM HTTP Server和Plug-Ins

使用与Centos7/RHEL7的64位环境

安装依赖

```纯文本
yum install gtk2.i686 libXtst.i686 libXp*.i686 libXt*.i686 libxtst*.i686 libcanberra-gtk2.i686 libcanberra-gtk3.i686 libcanberra-devel.i686 libgtk-x11-2.0.so.0 libpk-gtk-module.so libcanberra-gtk-module.so gtk2-devel-2.24.31-1.el7.x86_64 gtk2-devel-2.24.31-1.el7.i686
```

准备文件

```纯文本
WAS_ND_V8.5.5_1_OF_3.zip
WAS_ND_V8.5.5_2_OF_3.zip
WAS_ND_V8.5.5_3_OF_3.zip
agent.installer.linux.gtk.x86_64_1.6.2000.20130301_2248.zip
```

解压文件

-   对was压缩包，使用`unzip -oq`强制覆盖静默安装
-   对mgr安装包，使用`unzip -d 解压到指定目录`

解压InstalMgr1.6.2\_LNX\_X86\_WAS\_8.5.5.zip并执行安装安装

```纯文本
./installc -silent –acceptLicense
```

注：将安装到/opt/IBM/InstallationManager

> 如果没有图形化界面

```纯文本
yum -q  -y groupinstall "Server with GUI"
# 启动模式更改为图形化界面模式
systemctl set-default graphical.target
# 启动图形化界面
startx
```

以root用户执行下列程序

```纯文本
/opt/IBM/InstallationManager/eclipse/IBMIM
```

通过首选项，选择WAS8.5压缩包的repository.config，安装方法如同windows 的安装程序一般。

安装选项带上Sample applications即可。

安装结束后，弹出窗口。
