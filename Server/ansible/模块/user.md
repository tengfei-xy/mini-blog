创建账户

```
$ ansible all -m user -a "name=foo password=<crypted password here>"
```

删除用户

```
$ ansible all -m user -a "name=foo state=absent"
```

