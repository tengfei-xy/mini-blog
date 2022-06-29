# hashcat方式

> 参考文档
>
> https://zxacn.com/articles/2019/08/02/1564729501559.html
>
> https://dfine.tech/posts/b0550b46/

## 第一步：获取加密excel的hash值

下载office2john.py

```
https://github.com/magnumripper/JohnTheRipper/blob/bleeding-jumbo/run/office2john.py
```

在python环境下执行下列语句

```
python ./office2john.py ./20220.xlsx > hash
```

查看hash文件，可知该文件是2007版本的哈希类型

```
20220.xlsx:$office$*2007*20*128*16*96a52ba932667e90695f73bbc4f7dcd6*462d3fcf8c89eab60ec4205a25148ea1*e4d297b378d9d858c45d4050a85ee6a64434b06d
```

删除hash文件开头的文件名，仅保留如下内容并保存为ANSI编码的文件

```
$office$*2007*20*128*16*96a52ba932667e90695f73bbc4f7dcd6*462d3fcf8c89eab60ec4205a25148ea1*e4d297b378d9d858c45d4050a85ee6a64434b06d
```

## 第二步：破解

下载hashcat

```
# 首页
https://hashcat.net/hashcat/

# hashcat-6.2.5-windows
https://hashcat.net/files/hashcat-6.2.5.7z
```

根据上方哈希类型得到-m的参数（可以根据`.\hashget --help`来查看）

| -m 参数 | 说明                                            |
| ------- | ----------------------------------------------- |
| 9400    | MS Office 2007                                  |
| 9500    | MS Office 2010                                  |
| 9600    | MS Office 2013                                  |
| 25300   | MS Office 2016- SheetProtection                 |
| 9700    | MS Office <= 2003 $0/$1, MD5 + RC4              |
| 9710    | MS Office <= 2003 $0/$1, MD5 + RC4, collider #1 |
| 9720    | MS Office <= 2003 $0/$1, MD5 + RC4, collider #2 |
| 9810    | MS Office <= 2003 $3, SHA1 + RC4, collider #1   |
| 9820    | MS Office <= 2003 $3, SHA1 + RC4, collider #2   |
| 9800    | MS Office <= 2003 $3/$4, SHA1 + RC4             |

安装opencl环境

```
# 下载首页
https://www.intel.com/content/www/us/en/developer/articles/tool/opencl-drivers.html

# windows版opencl
https://registrationcenter-download.intel.com/akdlm/irc_nas/vcp/18758/w_opencl_runtime_p_2022.1.0.3787.exe
```

进入hashcat的安装文件夹，并开始破解，带上--show时直接在尾部显示密码

```bat
.\hashcat.exe -m 9400 .\hash  -a 3 --show
```

Hash参数解析

| 参数    | 说明                     |
| ------- | ------------------------ |
| -m 9400 | MS Office 2007的加密类型 |
| -a 3    | 暴力破解                 |
| --show  | 直接显示密码             |

