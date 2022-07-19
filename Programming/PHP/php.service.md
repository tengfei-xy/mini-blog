编辑/usr/lib/systemd/system/php.service

```
[Unit]
Description=php daemon

[Service]
Type=forking
User=monster
Group=monster
ExecStart=/usr/local/php-8.1.8/sbin/php-fpm -y /usr/local/php-8.1.8/etc/php-fpm.d/www.conf
KillMode=process
KillSignal=SIGQUIT
Restart=on-failure
RestartSec=42s
```

直接启动php.service

```
systemctl start php.service
```

