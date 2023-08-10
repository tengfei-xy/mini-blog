# service

确定启动

```纯文本
$ ansible webservers -m service -a "name=httpd state=started"
```

重启某个服务(译者注:可能是确认已重启的状态?)

```纯文本
$ ansible webservers -m service -a "name=httpd state=restarted"
```

确认停止

```纯文本
$ ansible webservers -m service -a "name=httpd state=stopped"
```
