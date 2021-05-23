## 基注册表目录

```
HKLM\SOFTWARE\ORACLE\
```

12c

```
HKLM\SOFTWARE\ORACLE\KEY_OraDB12Home1
```

## Powershell(cmdlet)

查看所有变量

```
PS > Get-Item HKLM:\SOFTWARE\ORACLE\KEY_OraDB12Home1
```

查看具体属性

```
PS > (Get-ItemProperty HKLM:\SOFTWARE\ORACLE\KEY_OraDB12Home1).ORALCE_SID
```

新建ORACLE_SID

```
PS C:\Users\Administrator> New-ItemProperty HKLM:\SOFTWARE\ORACLE\KEY_OraDB12Home1 -name ORACLE_DID -value oradbhb -PropertyType string
```

设定值

```
PS > Set-ItemProperty HKLM:\SOFTWARE\ORACLE\KEY_OraDB12Home1 -name ORACLE_SID -value 123
```

## cmd(reg)

查看所有

```
> reg query HKLM\SOFTWARE\ORACLE\KEY_OraDB12Home1
```

查看SID

```
> reg query HKLM\SOFTWARE\ORACLE\KEY_OraDB12Home1 /v ORACLE_SID
```

修改SID并启动

```
> reg add HKLM\SOFTWARE\ORACLE\KEY_OraDB12Home1 /f /v ORACLE_SID /d 123 & sqlplus / as sysdba
```

