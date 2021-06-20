# ssh免密
B 复制到 A  、B输入密码(B主机 免密码)
1. B进行ssh-keyen -t rsa(三次回车)
2. 复制 B 的\~/.ssh/id\_rsa.pub 追加到 A 的\~/.ssh/authorized\_keys

~/.ssh/authorized\_keys必须是600,可看/var/log/secure文件或ssh -vvv 进行调试