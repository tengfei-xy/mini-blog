# CREATE
## 创建用户
create user 'test'@'localhost' identified by 'password’;

## 创建表

```sql
CREATE TABLE IF NOT EXISTS `UserLogin`(
   `mail` VARCHAR(20) NOT NULL,
   `nickname` VARCHAR(5) NOT NULL,
   `userid` char(5) NOT NULL,
   `password` VARCHAR(20) NOT NULL,
   `admin` int(1) NOT NULL,
   `count` int(5) DEFAULT '0',
   `lasttime` TIMESTAMP NOT NULL,
   `lastip` VARCHAR(15) NOT NULL,
   PRIMARY KEY ( `mail` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

默认当前时间戳
DEFAULT CURRENT\_TIMESTAMP
```

```sql
CREATE TABLE IF NOT EXISTS `carid`(
`id` int(4) NOT NULL AUTO_INCREMENT,
  `carid` VARCHAR(7) NOT NULL,
  `name` VARCHAR(6),
  `telphone` char(11),
  `add_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `mod_time`  datetime DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY ( `id`,`carid` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8 AUTO_INCREMENT=1;
```





