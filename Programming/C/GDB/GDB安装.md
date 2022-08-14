启动GDB调试时，若出现Missing separate debuginfos, use: debuginfo-install

修改/etc/yum.repos.d/CentOS-Debuginfo.repo

```
[debug]
name=CentOS-7 - Debuginfo
baseurl=http://debuginfo.centos.org/7/$basearch/ 
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-Debug-7 
enabled=1
```

安装依赖glibc、yum-utils

```
sudo yum install -y gdb glibc yum-utils
```

安装glibc-debuginfo等包

```
sudo debuginfo-install -y glibc
```

