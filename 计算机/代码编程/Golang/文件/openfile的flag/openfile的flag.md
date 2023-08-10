# openfile的flag

import "os"

```golang
file, _ = os.OpenFile(file, os.O_RDWR|os.O_CREATE|os.O_APPEND, 0766)
```

第二参数的取值

```纯文本
    O_RDONLY int = syscall.O_RDONLY // 只读模式打开文件
    O_WRONLY int = syscall.O_WRONLY // 只写模式打开文件
    O_RDWR   int = syscall.O_RDWR   // 读写模式打开文件
    O_APPEND int = syscall.O_APPEND // 写操作时将数据附加到文件尾部
    O_CREATE int = syscall.O_CREAT  // 如果不存在将创建一个新文件
    O_EXCL   int = syscall.O_EXCL   // 和O_CREATE配合使用，文件必须不存在
    O_SYNC   int = syscall.O_SYNC   // 打开文件用于同步I/O
    O_TRUNC  int = syscall.O_TRUNC  // 如果可能，打开时清空文件
```

将文件读取到缓冲器

```golang
  file, _ := os.OpenFile(eventfile, os.O_RDONLY, 0766)
  var b []byte
  buf := bytes.NewBuffer(b)
  buf.ReadFrom(file)
  fmt.Println(buf.Bytes())
```
