# UPDATE
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值

当满足某个判断条件时，批量更新某列为关联表的对应列
语法：UPDATE 表1 t1 SET t1.字段值1 =(SELECT t2.字段值2 FROM 表2 t2 WHERE t1.关联字段1 = t2.关联字段2) WHERE 条件表达式;