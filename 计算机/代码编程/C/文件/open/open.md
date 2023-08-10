# open

## 目录

-   [原型](#原型)
-   [oflags](#oflags)
-   [mode](#mode)

# 原型

```c
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
int open(const char *path, int oflags);
int open(const char *path, int oflags, mode_t mode);
```

# oflags

头文件

```c
#include <fcntl.h>
```

可选值

| 模式        | 说明 |
| --------- | -- |
| O\_RDONLY | 只读 |
| O\_WRONLY | 已写 |
| O\_RDWR   | 读写 |

# mode

头文件

```c
#include <sys/stat.h>
```

可选值

| 模式       | 说明        |
| -------- | --------- |
| S\_IRUSR | 读权限，文件属主  |
| S\_IWUSR | 写权限，文件属主  |
| S\_IXUSR | 执行权限，文件属主 |
| S\_IRGRP | 读权限，文件属组  |
| S\_IWGRP | 写权限，文件属组  |
| S\_IXGRP | 执行权限，文件属组 |
| S\_IROTH | 读权限，其他用户  |
| S\_IWOTH | 写权限，其他用户  |
| S\_IXOTH | 执行权限，其他用户 |
