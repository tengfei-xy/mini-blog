# test

```sql
-- windows 建立表空间
-- create tablespace xy datafile 'C:\app\Administrator\oradata\xy.dbf' size 10m extent management local uniform size 128k segment space management auto nologging;

-- Linux 建立表空间
create tablespace xy datafile '/u01/app/oracle/xy.dbf' size 10m autoextend on next 10m extent management local uniform size 128k segment space management auto nologging;

-- 建立、赋权用户
create user orcl identified by orcl default tablespace xy quota unlimited on xy;
grant create session to orcl;
grant create table to orcl;
GRANT UNLIMITED TABLESPACE TO orcl;

-- 建立表
create table orcl.idt(id number(10),id2 number(10),id3 number(10));

-- 插入数据
insert into orcl.idt values(1,1,1);

-- 循环插入数据
begin 
for i in 1..1000 loop 
insert into orcl.idt values(i,i,i);
end loop; 
end; 
/
commit; 

-- 插入大量数据
begin 
for i in 1..10000000 loop 
insert into orcl.idt values(i,i,i);
if mod(i, 1000)=0 then 
commit; 
end if; 
end loop; 
end; 
/

-- 创建索引
create index  orcl.idx_idt_id on orcl.idt(id);
```
