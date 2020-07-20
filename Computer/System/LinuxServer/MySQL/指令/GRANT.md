# GRANT
SLAVE
仅用于复制目的的账号，只需要REPLICATION权限
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%.example.com’;