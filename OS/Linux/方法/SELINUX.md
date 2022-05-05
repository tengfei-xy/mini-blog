## 查看SELinux状态

### `/usr/sbin/sestatus -v`    

如果SELinux status参数为enabled即为开启状态

### `getenforce`        

也可以用这个命令检查

## 关闭SELinux：

临时关闭

```
setenforce 0
```

永久关闭

```shell
# 将SELINUX=enforcing改为SELINUX=disabled
vi /etc/selinux/config

# 重启
reboot
```

快速关闭

```
sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config && sduo setenforce 0
```



## 开启

setenforce 1

设置SELinux 成为enforcing模式