创建账户（前提是Successfully installed passlib-1.7.4）passlib-1.7.4-py2.py3-none-any.wh

```
$ ansible all -m user -a "name=foo password={{ oracle_user_password | password_hash('sha512') }}"
```

删除用户

```
$ ansible all -m user -a "name=foo state=absent"
```

