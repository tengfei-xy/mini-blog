# ssh免密

场景1：B 复制到 A  

场景2：B输入密码(B主机 免密码)

- 方法1

  1. B进行ssh-keyen -t rsa(三次回车)
  2. 复制 B 的\~/.ssh/id\_rsa.pub 追加到 A 的\~/.ssh/authorized\_keys

  注：~/.ssh/authorized\_keys必须是600,可看/var/log/secure文件或ssh -vvv 进行调试

- 方法2

  ```
  ssh-copy-id root@主机B
  ```

  注：提示/usr/bin/ssh-copy-id: ERROR: failed to open ID file '/root/.pub': 没有那个文件或目录时需要`ssh-keygen`

