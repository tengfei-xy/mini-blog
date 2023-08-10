# bing

bing返回值

| 值             | 含义                         | 备注               |
| ------------- | -------------------------- | ---------------- |
| EADDRINUSE    | 给定地址已经使用                   |                  |
| EBADF         | sockfd不合法                  |                  |
| EINVAL        | sockfd已经绑定到其他地址            |                  |
| ENOTSOCK      | sockfd是一个文件描述符，不是socket描述符 |                  |
| EACCES        | 地址被保护，用户的权限不足              |                  |
| EADDRNOTAVAIL | 接口不存在或者绑定地址不是本地            | UNIX协议族，AF\_UNIX |
| EFAULT        | my\_addr指针超出用户空间           | UNIX协议族，AF\_UNIX |
| EINVAL        | 地址长度错误，或者socket不是AF\_UNIX族 | UNIX协议族，AF\_UNIX |
| ELOOP         | 解析my\_addr时符号链接过多          | UNIX协议族，AF\_UNIX |
| ENAMETOOLONG  | my\_addr过长                 | UNIX协议族，AF\_UNIX |
| ENOENT        | 文件不存在                      | UNIX协议族，AF\_UNIX |
| ENOMEN        | 内存内核不足                     | UNIX协议族，AF\_UNIX |
| ENOTDIR       | 不是目录                       | UNIX协议族，AF\_UNIX |
| EROFS         | socket节点应该在制度文件系统上         | UNIX协议族，AF\_UNIX |
