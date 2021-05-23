原理：

```
[root@ora log]# echo $UID
0
```

实现：

```
if [ "$UID" -ne "0"]
```

