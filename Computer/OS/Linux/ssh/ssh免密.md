# ssh免密
B 复制到 A  (B主机 免密码)
1. B进行ssh-keyen -t rsa(三次回车)
2. 复制 B 的\~/.ssh/id\_rsa.pub 追加到 A 的\~/.ssh/authorized\_keys

