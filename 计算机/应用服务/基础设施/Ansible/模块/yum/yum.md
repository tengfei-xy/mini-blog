# yum

确认一个软件包已经安装,但不去升级它

```纯文本
ansible webservers -m yum -a "name=acme state=present"
```

确认一个软件包的安装版本:

```纯文本
$ ansible webservers -m yum -a "name=acme-1.5 state=present"
```

确认一个软件包还没有安装:

```纯文本
$ ansible webservers -m yum -a "name=acme state=absent"
```
