# firewall-cmd

[参考链接](https://juejin.cn/post/6844904162988130312)

重新载入

```bash
firewall-cmd --reload
```

查看端口

```bsah
firewall-cmd --zone=public --query-port=80/tcp
```

删除端口

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

添加端口

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

查看所有开放的端口

```bash
firewall-cmd --list-ports
```

禁止ping

```bash
firewall-cmd --permanent --add-rich-rule='rule protocol value=icmp drop'
firewall-cmd --permanent --remove-rich-rule='rule protocol value=icmp drop'
```

创建zone

```bash
firewall-cmd --new-zone=<zone>
```

启用zone

```bash
firewall-cmd --set-default-zone=aliyun
```

针对具体ip和端口

```bash
firewall-cmd --permanent --add-rich="rule family="ipv4" source address="183.135.155.40" port protocol="tcp" port="3306" accept"
```

