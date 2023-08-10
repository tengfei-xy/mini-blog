# 查看segment

select sum(bytes)/1024/1024/1024 from dba\_segments where owner='ORCL\_USER';
