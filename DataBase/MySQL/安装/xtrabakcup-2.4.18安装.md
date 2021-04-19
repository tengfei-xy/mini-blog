```shell
# 下载安装文件
wget http://tools.file.irybd.com/percona-xtrabackup-24-2.4.18-1.el7.x86_64.rpm

# 安装依赖
wget ftp://rpmfind.net/linux/atrpms/el6-x86_64/atrpms/stable/libev-4.04-2.el6.x86_64.rpm
rpm -ivh libev-4.04-2.el6.x86_64.rpm
yum -y install perl perl-devel libaio libaio-devel perl-Time-HiRes perl-DBD-MySQL libaio rsync perl l perl-Digest-MD5

# 安装xtrabackup
rpm -ivh percona-xtrabackup-24-2.4.18-1.el7.x86_64.rpm
```

