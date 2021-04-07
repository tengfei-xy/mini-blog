# CREATE
## 创建用户
create user 'test'@'localhost' identified by 'password’;

## 创建表
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

