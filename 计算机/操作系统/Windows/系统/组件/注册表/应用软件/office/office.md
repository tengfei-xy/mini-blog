# office

## 目录

-   [软件安装位置](#软件安装位置)
-   [密钥位置（二进制）](#密钥位置二进制)

## 软件安装位置

在注册表展开

```纯文本
HKEY_CLASSES_ROOT\Installer\Products\
```

删除00002、00005、00006开头的项即可

## 密钥位置（二进制）

key为DiagitalProductId

```纯文本
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion
```
