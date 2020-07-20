# SHOW
## tables
查看库中的表
show tables;
## global
查看当前数据库最大连接数
show global status like 'Max_used_connections'; 

## status
查看进程连接情况
show status like 'Threads%';