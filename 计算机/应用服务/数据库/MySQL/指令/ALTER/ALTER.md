# ALTER

## 目录

-   [修改表格式](#修改表格式)
-   [添加字段](#添加字段)
    -   [在末尾添加字段](#在末尾添加字段)
-   [修改字段名](#修改字段名)
-   [修改表名](#修改表名)
-   [修改主键](#修改主键)

## 修改表格式

ALTER TABLE UserLogin MODIFY userid char(15) NULL;

## 添加字段

ALTER TABLE UserLogin ADD cookie varchar(30) NOT NULL;

### 在末尾添加字段

ALTER TABLE <表名> ADD <新字段名> <数据类型> \[约束条件] AFTER <已经存在的字段名>;

## 修改字段名

ALTER  TABLE 表名 CHANGE cookie status varchar(30);

## 修改表名

alter table UserLogin rename UserState;

## 修改主键

如果表已经存在，则删除旧的主键：

```sql
ALTER TABLE yourtable
DROP PRIMARY KEY;
```

并重新创建它：

```sql
ALTER TABLE yourtable
ADD PRIMARY KEY (employeeid, recordmonth, recordyear);
```
