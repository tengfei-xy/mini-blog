# SHOW

## 目录

-   [tables](#tables)
-   [global](#global)
-   [status](#status)

## tables

查看库中的表
show tables;

## global

查看当前数据库最大连接数
show global status like 'Max\_used\_connections';

## status

查看进程连接情况
show status like 'Threads%';
