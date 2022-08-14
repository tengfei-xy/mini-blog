PasswordAuthentication no :不允许使用密码登录

PermitRootLogin yes：允许root登录

PubkeyAuthentication yes ：允许公钥认证

RSAAuthentication yes ：开启RSA认证

DenyUsers oracle：禁止oracle用户登录（手动添加配置）

UsePAM yes：使用pam认证模块

> 关于UsePAM说明：
>
> 1. 如果yes，但对应的pam配置文件不存在，登录则会提示Permission denied。
>
> 2. 如果编译时未添加--with-pam选项，则需要注释该选项。
