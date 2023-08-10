# postfix3安装

## 目录

-   [yum方式安装postfix3](#yum方式安装postfix3)

# yum方式安装postfix3

<https://www.ryadel.com/en/postfix-3-install-setup-linux-centos-for-sending-mail-smtp-smtps-starttls/>

`vi /etc/yum.repos.d/gf.repo`

```ini
[gf]
name=Ghettoforge packages that won't overwrite core distro packages.
mirrorlist=http://mirrorlist.ghettoforge.org/el/7/gf/$basearch/mirrorlist
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-gf.el7
failovermethod=priority

[gf-plus]
name=Ghettoforge packages that will overwrite core distro packages.
mirrorlist=http://mirrorlist.ghettoforge.org/el/7/plus/$basearch/mirrorlist
# Please read http://ghettoforge.org/index.php/Usage *before* enabling this repository!
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-gf.el7
failovermethod=priority

```

```纯文本
wget https://mirror.ghettoforge.org/distributions/gf/RPM-GPG-KEY-gf.el7
sudo cp RPM-GPG-KEY-gf.el7 /etc/pki/rpm-gpg
sudo yum install postfix3
```

\`\`
