# ext4查看文件创建时间

## 目录

-   [步骤](#步骤)
-   [脚本](#脚本)

仅支持ext4的文件系统

# 步骤

以test01.txt文件为例，输出文件的inode号

```bash
stat -c %i test01.txt
```

查看文件所在的挂载点

```bash
df test01.txt | sed -n '2{s/ .*$//;p}'
```

最后通过debugfs就可以查询到文件的完整信息了

```awk
debugfs -R 'stat <1839472>' /dev/mapper/centos-root
```

# 脚本

```纯文本
file=./data
inode=$(stat -c %i "$file")
mount_path=$(df $file | sed -n '2{s/ .*$//;p}')
debugfs -R "'stat <$inode>'" "$mount_path"
```
