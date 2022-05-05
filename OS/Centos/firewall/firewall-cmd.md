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

添加富规则针对单个ip和端口（如果有个IP或端口需要重复使用命令）

```bash
# 单个IP和单个端口
firewall-cmd --permanent --add-rich="rule family="ipv4" source address="183.135.155.40" port protocol="tcp" port="3306" accept"
# 单个IP和多个端口
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="20.115.226.155" port protocol="tcp" port="1-65535" accept"
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="10.77.12.4/22" port protocol="tcp" port="50001" accept"
firewall-cmd --reload
```

删除富规则

```bash
firewall-cmd --permanent --remove-rich="rule family="ipv4" source address="183.135.155.40" port protocol="tcp" port="50001" accept"
firewall-cmd --reload
```

查看富规则

```bash
firewall-cmd --permanent --list-rich
```

