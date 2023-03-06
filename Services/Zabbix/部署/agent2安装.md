# Windows

[下载地址](https://www.zabbix.com/cn/download_agents)

[官方参考手册](https://www.zabbix.com/documentation/6.2/en/manual/installation/install_from_packages/win_msi)



安装命令

```
SET INSTALLFOLDER=C:\Program Files\za
msiexec /l*v log.txt /i zabbix_agent-6.2.0-x86.msi /qn^
 LOGTYPE=file^
 LOGFILE="%INSTALLFOLDER%\za.log"^
 SERVER=192.168.6.76^
 LISTENPORT=12345^
 SERVERACTIVE=::1^
 HOSTNAME=myHost^
 TLSCONNECT=psk^
 TLSACCEPT=psk^
 TLSPSKIDENTITY=MyPSKID^
 TLSPSKFILE="%INSTALLFOLDER%\mykey.psk"^
 TLSCAFILE="c:\temp\f.txt1"^
 TLSCRLFILE="c:\temp\f.txt2"^
 TLSSERVERCERTISSUER="My CA"^
 TLSSERVERCERTSUBJECT="My Cert"^
 TLSCERTFILE="c:\temp\f.txt5"^
 TLSKEYFILE="c:\temp\f.txt6"^
 ENABLEPATH=1^
 INSTALLFOLDER="%INSTALLFOLDER%"^
 SKIP=fw^
 ALLOWDENYKEY="DenyKey=vfs.file.contents[/etc/passwd]"
```

配置参考

```
PidFile=C:\Program Files\Zabbix Agent 2\zabbix_agentd.pid
LogFile=C:\Program Files\Zabbix Agent 2\zabbix_agentd.log
DebugLevel=0
Server=192.168.0.25
Hostname=Test-winddows
AllowRoot=0
User=zabbix
UnsafeUserParameters=1
UserParameter=zabbix_shell[*],/usr/local/services/zabbix-6.2.6/etc/zabbix_agentd.conf.d/zabbix_shell.sh $1 $2 $3
```

