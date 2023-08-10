# 禁用ipv6

`sudo vi /etc/default/grub`
输入：GRUB\_CMDLINE\_LINUX="ipv6.disable=“
`grub2-mkconfig -o /boot/grub2/grub.cfg``reboot`
