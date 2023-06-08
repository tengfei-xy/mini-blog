在CentOS 9中，nslookup命令已经被替换为host命令。因此，您可以使用以下命令来安装host命令：

```
sudo dnf install bind-utils
```

安装完成后，您可以使用以下命令来执行DNS查询：

```
host example.com
```