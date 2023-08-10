# mysqldump

## 目录

-   [mysqldump备份](#mysqldump备份)

# mysqldump备份

mysqldump命令

```纯文本
-d 只备份结构 
-s 只备份数据
-T 数据和表分离备份
--compact 输出更紧缩格式的SQL语句
--compress 启用压缩功能,但会增加CPU负担
-F 根据binlog备份
-x 备份时锁定表
--no-data 不导出任何数据内容
--skip-add-indexes 跳过索引
--skip-comments 跳过注释行
```

示例

备份wordpress库

```纯文本
bin/mysqldump -B wordpress -u root -p | gzip > backup/wordpress_12_25.sql.gz
```

备份wordpress库中wp表

```纯文本
bin/mysqldump wordpress wp -u root -p | gzip > backup/wordpress_12_25.sql.gz
```

备份用户和权限

```纯文本
mysqldump -u [username] -p --no-data --skip-comments --compact mysql user > user.sql
```

针对压缩文件的数据恢复
zcat .sql.gz > .sql
mysql < .sql

利用binlog增量恢复
mysqlbinlog .000008 .000009 > .sql
mysql < .sql
