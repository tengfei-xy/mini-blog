# zabbix_get

> 无法用于简单监测，JAVA

> 该命令只能在server端使用，否则报错Check access restrictions in Zabbix agent configuration

调用键值

```bash
bin/zabbix_get -s 192.168.0.35 -k agent.hostname
```

调用参数，包含参数

```bash
zabbix_get -s 192.168.0.21  -k zabbix_shell[mysql,qps]
```

