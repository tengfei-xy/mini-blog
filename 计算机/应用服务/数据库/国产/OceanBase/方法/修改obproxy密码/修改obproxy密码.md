# 修改obproxy密码

以root\@proxysys账号登录OBProxy，修改root\@proxysys的密码，初始密码为空。

```sql
mysql -h127.0.0.1 -P2883 -uroot@proxysys
mysql> alter proxyconfig set obproxy_sys_password = 'bb123456';
```
