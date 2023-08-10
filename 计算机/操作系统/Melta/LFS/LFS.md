# LFS

## 目录

-   [版本](#版本)
    -   [11.0 LFS注意事项](#110-LFS注意事项)
-   [chroot](#chroot)
    -   [引导](#引导)
-   [control](#control)

# 版本

<https://www.linuxfromscratch.org/blfs/view/stable/>

<https://bf.mengyan1223.wang/lfs/zh_CN/11.0/>

<https://bf.mengyan1223.wang/lfs/zh_CN/9.0/>

<https://www.linuxfromscratch.org>

ID:29123

## 11.0 LFS注意事项

1.  chroot之后编译安装gettext之前，应该先编译ncurses添加--enable-shared在chroot下编译一次。
2.  部分软件如果`make check`失败，且和官方提供的日志不一致，且之前编译过一次，应该先删除软件，重新解包并编译。

# chroot

```纯文本
mknod -m 600 $LFS/dev/console c 5 1
mknod -m 666 $LFS/dev/null c 1 3
mount -v --bind /dev $LFS/dev
mount -v --bind /dev/pts $LFS/dev/pts
mount -vt proc proc $LFS/proc
mount -vt sysfs sysfs $LFS/sys
mount -vt tmpfs tmpfs $LFS/run
if [ -h $LFS/dev/shm ]; then
  mkdir -pv $LFS/$(readlink $LFS/dev/shm)
fi

chroot "$LFS" /usr/bin/env -i          \
    HOME=/root TERM="$TERM"            \
    PS1='(lfs chroot) \u:\w\$ '        \
    PATH=/usr/bin:/usr/sbin            \
    /bin/bash --login

```

反chroot

```纯文本
umount $LFS/dev{/pts,}
umount $LFS/{sys,proc,run}
```

## 引导

1.  使用 UEFI 引导 LFS，按照 [BLFS 页面](https://www.linuxfromscratch.org/blfs/view/11.0/postlfs/grub-efi.html "BLFS 页面")中的说明，安装支持 UEFI 的 GRUB。
2.  使用 GRUB 引导 LFS，按照[GRUB 设定引导过程](https://bf.mengyan1223.wang/lfs/zh_CN/11.0/chapter10/grub.html "GRUB 设定引导过程")中的说明。

# control

-   对于中文，日文，韩文，以及其他一些语言文字，无论如何配置 Linux 控制台，都不可能正常显示所需要的字符。这些语言的用户应该安装 X 窗口系统，能够覆盖所需要的字符的字体，以及合适的输入法 (如 SCIM 支持许多语言的输入)。`/etc/sysconfig/console` 文件只控制 Linux 字符控制台的本地化。它和 X 窗口系统，ssh 连接，或者串口终端中的键盘布局设置和终端字体毫无关系。在这些情况下，不存在上述的两项限制。

grub.cfg

```bash
# Begin /boot/grub/grub.cfg
set default=0
set timeout=5
insmod ext2
#set root=(hd0,1)

menuentry "GNU/Linux, Linux 5.13.12-lfs-11.0" {
search --no-floppy --fs-uuid --set 70240ee0-0fc0-4ad2-a82e-c359b02df80b
linux   /boot/vmlinuz-5.13.12-lfs-11.0 root=UUID=70240ee0-0fc0-4ad2-a82e-c359b02df80b ro
}

```
