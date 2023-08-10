# 自定义SQL提示符

在.bash\_profile里添加

```shell
export SQLPATH=$HOME/scripts
```

生效.bash\_profile

```shell
. ~/.bashr_profile
```

在\$HOME/scripts/login.sql下输入对应的提示符指令

```shell
echo "SET SQLPROMPT '&_USER.@&_CONNECT_identifier.> '">$HOME/scripts/login.sql
```

注：logsin.sql文件名不能改变

| 变量                    | 作用               |
| --------------------- | ---------------- |
| \_CONNECT\_IDENTIFIER | 连接标识符,如数据库的SID   |
| \_DATA                | 当前日期             |
| \_EDITOR              | SQL EDIT命令使用的编辑器 |
| \_O\_VERSION          | Oracle的版本号       |
| \_O\_RELEASE          | Oracle的发行号       |
| \_PRIVILEGE           | 当前连接会话的权限等级      |
| \_SQLPLUS\_RELEASE    | SQL\*PLUS的发行号    |
| \_USER                | 当前使用的用户          |
