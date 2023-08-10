# SELINUX

## 目录

-   [查看SELinux状态](#查看SELinux状态)
-   [关闭SELinux](#关闭SELinux)
-   [开启SELinux](#开启SELinux)

## 查看SELinux状态

```bash
# 方法一，如果SELinux status参数为enabled即为开启状态
/usr/sbin/sestatus -v
# 方法二
getenforce
```

## 关闭SELinux

临时关闭

```纯文本
setenforce 0
```

永久关闭（重启生效）

```bash
# 将SELINUX=enforcing改为SELINUX=disabled
vi /etc/selinux/config
```

快速关闭

```纯文本
sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config && sduo setenforce 0
```

## 开启SELinux

setenforce 1

设置SELinux 成为enforcing模式
