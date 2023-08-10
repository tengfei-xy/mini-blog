# host

在CentOS 9中，nslookup命令已经被替换为host命令。因此，您可以使用以下命令来安装host命令：

```纯文本
sudo dnf install bind-utils
```

安装完成后，您可以使用以下命令来执行DNS查询：

```纯文本
host example.com
```
