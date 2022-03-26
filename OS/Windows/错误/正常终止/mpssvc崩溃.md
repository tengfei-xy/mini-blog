# mpssvc崩溃

在regedit中依次展开如下路径

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\
```

对SharedAccess项,添加everyone的读写权限。



扩展；更具体的位置

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Epoch
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Epoch2
```

