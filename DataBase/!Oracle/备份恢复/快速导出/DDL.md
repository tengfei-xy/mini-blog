预置语句

```
set long 99999         
set pagesize 1000
```

## 表

- 单个表

  ```sql
  -- ('TABLE','表名','用户名'') 
  SELECT DBMS_METADATA.GET_DDL('TABLE','IDT','TENGFEI') FROM DUAL;
  ```

- sysdba下单个用户的所有表

  ```
  SELECT DBMS_METADATA.GET_DDL('TABLE', u.tablename)
  FROM DBA_TABLES u
  where u.owner='TENGFEI';
  ```

  

## 索引

- 单个索引

  ```sql
  select dbms_metadata.get_ddl('INDEX','PK_DEPT','SCOTT') from dual;
  ```

- 用户的所有索引

  ```sql
  select dbms_metadata.get_ddl('INDEX',index_name)
  from dba_indexes a where a.owner='TENGFEI';
  ```

  

## 表空间

- 单个表空间

  ```sql
  SELECT DBMS_METADATA.GET_DDL('TABLESPACE', TS.tablespace_name) 
  FROM DBA_TABLESPACES TS where ts.tablespace_name='XY';
  ```

  

## 用户

- sysdba下所有用户

  ```sql
  SELECT DBMS_METADATA.GET_DDL('USER',U.username)
  FROM DBA_USERS;
  ```

- sysdba下的单个用户

  ```sql
  SELECT DBMS_METADATA.GET_DDL('USER',username)
  FROM DBA_USERS WHERE USERNAME='TENGFEI';
  ```

  

## 混合

- 一个用户下的所有表，索引，存储过程的ddl

  ```sql
  SELECT DBMS_METADATA.GET_DDL(U.OBJECT_TYPE, u.object_name)
  FROM USER_OBJECTS u
  where U.OBJECT_TYPE IN ('TABLE','INDEX','PROCEDURE');
  ```

  

