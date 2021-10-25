# 原型

```c
#include <fcntl.h>
#include <sys/types.h>
#include <sys/stat.h>
int open(const char *path, int oflags);
int open(const char *path, int oflags, mode_t mode);
```

# oflags

| 模式     | 说明 |
| -------- | ---- |
| O_RDONLY | 只读 |
| O_WRONLY | 已写 |
| O_RDWR   | 读写 |

# mode

定义在sys/stat.h

| 模式    | 说明               |
| ------- | ------------------ |
| S_IRUSR | 读权限，文件属主   |
| S_IWUSR | 写权限，文件属主   |
| S_IXUSR | 执行权限，文件属主 |
| S_IRGRP | 读权限，文件属组   |
| S_IWGRP | 写权限，文件属组   |
| S_IXGRP | 执行权限，文件属组 |
| S_IROTH | 读权限，其他用户   |
| S_IWOTH | 写权限，其他用户   |
| S_IXOTH | 执行权限，其他用户 |

