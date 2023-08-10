# pam\_echo

将下列内容添加到/etc/pam.d/sshd，表示在用户在进行身份验证时，将输出指定内容

用法1：输出信息来自指定内容

```bash
auth optional pam_echo.so 从%H登录到%h,使用的服务为%s,使用的tty为%t,使用的远程用户是%U,使用的本地用户是%u
```

用法2：输出信息来自文件

```bash
auth optional pam_echo.so file=/tmp/233
```

用法查询 man pam\_echo
