# repo

7.5

```纯文本
cat > /etc/yum.repos.d/redhat-7.5.repo << EOF
[base]
name=CentOS-$releasever - Base
baseurl=http://vault.centos.org/7.5.1804/os/$basearch/
gpgcheck=1
gpgkey=http://vault.centos.org/7.5.1804/os/x86_64/RPM-GPG-KEY-CentOS-7


#released updates
[updates]
name=CentOS-$releasever - Updates
baseurl=http://vault.centos.org/7.5.1804/updates/$basearch/
gpgcheck=1
gpgkey=http://vault.centos.org/7.5.1804/os/x86_64/RPM-GPG-KEY-CentOS-7


[extras]
name=CentOS-$releasever - Extras
baseurl=http://vault.centos.org/7.5.1804//extras//$basearch/
gpgcheck=1
gpgkey=http://vault.centos.org/7.5.1804/os/x86_64/RPM-GPG-KEY-CentOS-7

[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://vault.centos.org/7.5.1804/centosplus//$basearch/
gpgcheck=1
EOF
```
