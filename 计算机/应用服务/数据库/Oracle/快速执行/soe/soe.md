# soe

```sql
create tablespace SOE datafile '/u01/app/oracle/soe.dbf' size 100m autoextend on next 10m extent management local uniform size 10m segment space management auto nologging;

create user soe identified by soe default tablespace soe quota unlimited on soe;
grant create session to soe;
grant create table to soe;
GRANT UNLIMITED TABLESPACE TO soe;
```
