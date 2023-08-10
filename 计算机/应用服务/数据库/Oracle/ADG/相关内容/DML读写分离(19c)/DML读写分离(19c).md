# DML读写分离(19c)

DML Redirecition：将偶然发送到ADG上的DML操作，自动转发到主库执行，然后通过主库日志传递到备库实时应用，在保证了ACID的前提下，大大增强了备库的实用性，这被称为 DML Redirection 。

<https://www.modb.pro/db/754>

<http://blog.itpub.net/26736162/viewspace-2656071/>
