# mysqldump备份
备份wordpress库
bin/mysqldump -B wordpress -u root -p | gzip > backup/wordpress\_12\_25.sql.gz

备份wordpress库中wp表
bin/mysqldump wordpress wp -u root -p | gzip > backup/wordpress\_12\_25.sql.gz

-d 只备份结构 
-s 只备份数据
-T 数据和表分离备份
--compact 减少输出信息
-F 根据binlog备份
-x 备份时锁定表

针对压缩文件的数据恢复
zcat .sql.gz > .sql
mysql < .sql

利用binlog增量恢复
mysqlbinlog .000008 .000009 > .sql
mysql < .sql