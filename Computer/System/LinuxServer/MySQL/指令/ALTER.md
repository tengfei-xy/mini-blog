# ALTER
修改表格式
ALTER TABLE UserLogin MODIFY userid char(15) NULL;

添加字段
ALTER TABLE UserLogin ADD cookie varchar(30) NOT NULL;

修改字段名
ALTER  TABLE 表名 CHANGE cookie status varchar(30);

修改表名
alter table UserLogin rename UserState;