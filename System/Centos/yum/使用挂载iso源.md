```shell
mkdir /mnt/iso
mount -o /dev/sr0/ !$
vi /etc/yum.repos.d/local.repo

[local]
name=local
# baseurl是挂载目录
baseurl=file:///mnt/iso
enabled=1
gpgcheck=0
# baseurl里可以查看key
gpgkey=file:///mnt/iso/RPM-GPG-KEY-CentOS-7
```

