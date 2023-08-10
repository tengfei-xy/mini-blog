# socket

## 目录

-   [domain - 协议族](#domain---协议族)
-   [type 数据传输类型](#type-数据传输类型)
    -   [SOCK\_STREAM](#SOCK_STREAM)
    -   [SOCK\_DGRAM](#SOCK_DGRAM)
-   [protocol 协议信息](#protocol-协议信息)

```c
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>

int socket(int domain, int type, int protocol);
```

# domain - 协议族

| 名称         | 协议族名称        |
| ---------- | ------------ |
| PF\_INET   | IPv4         |
| PF\_INET6  | IPv6         |
| PF\_LOCAL  | 本地通信的UNIX协议族 |
| PF\_PACKET | 底层套接字的协议族    |

# type 数据传输类型

## SOCK\_STREAM

说明：是一种面向连接的套接字

特点：数据不会丢失、按顺序传输、传输的数据不存在数据边界

> read()读取多次后，将读取完成，并返回0

## SOCK\_DGRAM

说明：是一种面向消息的套接字

特点：强调快速传输，而非传输顺序、传输的数据可能丢失，传输的数据有边界，限制每次传输的数据大小。

# protocol 协议信息

传递前两个参数即可创建所需套接字。所以大部分情况下可以向第三个参数 传递0，除非遇到以下这种情况:“同一协议族中存在多个数据传输方式相同的协议”。数据传输 方式相同，但协议不同。此时需要通过第三个参数具体指定协议信息。

-   创建IPv4协议族中面向连接套接字
    参数PF\_INET指IPv4网络协议族，SOCK\_STREAM是面向连接的数据传输方式。满足这2个条件的协议只有IPROTO\_TCP。这种套接字称为TCP套接字。
-   创建IPv4协议族中面向消息套接字
    参数PF\_INET指IPv4网络协议族，SOCK\_DGRAM是面向消息的数据传输方式。满足这两个条件的协议只有IPPPROTO\_UDP。这种套接字成为UDP套接字。
