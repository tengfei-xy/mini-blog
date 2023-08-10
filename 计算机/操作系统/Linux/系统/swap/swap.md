# swap

## 目录

-   [开启swap分区](#开启swap分区)
    -   [使用交换文件](#使用交换文件)
    -   [使用交换分区](#使用交换分区)

# 开启swap分区

-   使用交换文件
    这种方式适用于，已经装完系统但是没有多余分区可以用来做swap分区。
-   使用交换分区
    这种方式适用于有多余的分区可以用来做swap分区。

另外，对于swap空间划分多少，这个可以参考这篇文章[交换分区(swap)的大小分配](http://smilejay.com/2012/06/swap_size/ "交换分区(swap)的大小分配")。

## 使用交换文件

1.  创建swap文件
    ```bash
    dd if=/dev/zero of=/myswap bs=1M count=4096
    ```
2.  设置可访问权限
    ```bash
    chmod 600 /myswap
    ```
3.  格式化文件
    ```bash
    mkswap /myswap
    ```
4.  激活swap空间(每次swapon可以叠加swap空间)
    ```bash
    swapon /myswap
    ```
5.  开机自动启用swap空间，追加下面内容到/etc/fstab
    ```bash
    echo "/myswap swap swap default 0 0" >>/etc/fstab
    ```

## 使用交换分区

```纯文本
> mkswap /dev/sda2
> swapon /dev/sda2
在/etc/fstab中添加下面这一句
/dev/sda2 none swap default 0 0
```
