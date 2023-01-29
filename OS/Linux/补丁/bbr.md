快速安装

```bash
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

```bash
sudo yum --enablerepo=elrepo-kernel install kernel-lt
sudo awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
sudo grub2-set-default 0
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo sh -c 'echo -e "
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
" >> /etc/sysctl.conf'
sudo sysctl -p
```



模块启动判断：

```
lsmod | grep bbr && sudo yum -y -q install wget
```
