# waitpid

## 目录

-   [定义](#定义)
-   [参数](#参数)
    -   [pid\_t pid](#pid_t-pid)
    -   [int \* status](#int--status)
    -   [int options](#int-options)
-   [返回值](#返回值)

# 定义

```c
#include <sys/types.h> 
#include <sys/wait.h>
pid_t waitpid(pid_t pid,int *status,int options);
```

# 参数

## pid\_t pid

| 参数值    | 说明                                                     |
| ------ | ------------------------------------------------------ |
| pid<-1 | 等待进程组号为pid绝对值的任何子进程。                                   |
| pid=-1 | 等待任何子进程，此时的waitpid()函数就退化成了普通的wait()函数。                |
| pid=0  | 等待进程组号与目前进程相同的任何子进程，也就是说任何和调用waitpid()函数的进程在同一个进程组的进程。 |
| pid>0  | 等待进程号为pid的子进程。                                         |

## int \* status

该参数作为子进程的状态。如果在函数结束后不需要获取子进程为什么退出，可以传入NULL。

| 参数值                 | 说明                                            |
| ------------------- | --------------------------------------------- |
| WIFEXITED(status)   | 如果子进程正常结束，它就返回真；否则返回假。                        |
| WEXITSTATUS(status) | 为真，则可以用该宏取得子进程exit()返回的结束代码。                  |
| WIFSIGNALED(status) | 如果子进程因为一个未捕获的信号而终止，它就返回真；否则返回假。               |
| WTERMSIG(status)    | 如果WIFSIGNALED(status)为真，则可以用该宏获得导致子进程终止的信号代码。 |
| WIFSTOPPED(status)  | 如果当前子进程被暂停了，则返回真；否则返回假。                       |
| WSTOPSIG(status)    | 如果WIFSTOPPED(status)为真，则可以使用该宏获得导致子进程暂停的信号代码。 |

## int options

该参数是控制waitpid()函数的行为

| 参数        | 说明                                                                 |
| --------- | ------------------------------------------------------------------ |
| WNOHANG   | 如果pid指定的子进程没有结束，则waitpid()函数立即返回0，而不是阻塞在这个函数上等待；如果结束了，则返回该子进程的进程号。 |
| WUNTRACED | 如果子进程进入暂停状态，则马上返回。                                                 |
| 0         | 不使用任何参数                                                            |

# 返回值

执行成功返回进程号。如有错误返回-1，并且将失败的原因存放在errno变量中。
