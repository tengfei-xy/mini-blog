# 快速格式化U盘

查找IDENTIFIER

```bash
diskutil list
```

格式化

```纯文本
format：
UFSD：NTFS
MS-DOS：

分区类型：
MBR

diskutil eraseDisk  <format> "<U盘名称>" <分区类型> <IDENTIFIER>
```
