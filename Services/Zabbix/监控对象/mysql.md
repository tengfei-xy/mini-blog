编辑/usr/lib/systemd/system/zabbix_agentd.service

```ini
[Unit]
Description=Zabbix Agentd
After=syslog.target
After=network.target
 
[Service]
Type=forking
PIDFile=/tmp/zabbix_agentd.pid
Restart=on-failure
KillMode=control-group
ExecStart=/usr/local/services/zabbix-6.2.6/sbin/zabbix_agentd -c /usr/local/services/zabbix-6.2.6/etc/zabbix_agentd.conf
ExecStop=/bin/kill -SIGTERM $MAINPID
RestartSec=10s
 
[Install]
WantedBy=multi-user.target
```



创建mysql用户

```sql
create user 'zabbix'@'localhost' identified with mysql_native_password by '6NE80@0U<?P[$p$aqw';
GRANT SELECT, PROCESS, SUPER, REPLICATION CLIENT ON *.* TO 'zabbix'@'localhost';
flush privileges;
```

