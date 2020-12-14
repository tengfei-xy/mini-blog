# 开启swap分区



- 使用交换文件
  这种方式适用于，已经装完系统但是没有多余分区可以用来做swap分区。
- 使用交换分区
  这种方式适用于有多余的分区可以用来做swap分区。

另外，对于swap空间划分多少，这个可以参考这篇文章[交换分区(swap)的大小分配](http://smilejay.com/2012/06/swap_size/)。

## 使用交换文件

a.创建交换文件

```
> fallocate -l 4G /swap
或
> dd if=/dev/zero of=/swap bs=1M count=4096123
```

b.设置可访问权限
`> chmod 600 /swapfile`

c.格式化文件
`> mkswap /swapfile`

d.激活swap空间
`> swapon /swapfile`

f.开机自动启用swap空间
编辑/etc/fstab，添加下面这一句
`/swapfile none swap default 0 0`

## 使用交换分区

```
> mkswap /dev/sda2
> swapon /dev/sda2
在/etc/fstab中添加下面这一句
/dev/sda2 none swap default 0 0
```