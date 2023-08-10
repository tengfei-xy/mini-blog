# 是否为root用户

原理：

```纯文本
[root@ora log]# echo $UID
0
```

实现：

```纯文本
if [ "$UID" -ne "0"]
```
