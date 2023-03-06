[RHEL Root on ZFS](https://openzfs.github.io/openzfs-docs/Getting Started/RHEL-based distro/RHEL-based distro Root on ZFS.html)

[编译参考](https://openzfs.github.io/openzfs-docs/Developer%20Resources/Building%20ZFS.html)

```
yum install -y libtool* autoconf automake zlib-devel libuuid-devel libblkid-devel krb5-devel kernel-devel
wget http://rpmfind.net/linux/centos/7.9.2009/updates/x86_64/Packages/openssl-devel-1.0.2k-25.el7_9.x86_64.rpm
rpm -ivh ./openssl-devel-1.0.2k-25.el7_9.x86_64.rpm 

```

```
wget https://github.com/openzfs/zfs/archive/refs/tags/zfs-2.1.9.tar.gz
./autogen.sh
./configure --prefix=/usr/local/services/zfs-2.1.9
make -j4 
make install
```

