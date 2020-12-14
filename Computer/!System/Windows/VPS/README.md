# 一键初始化windows端vps代理服务器的脚本使用方法

## 远程连接

mac端使用microsoft Remote Desktop远程vps服务器，同时配置重定向文件夹将**本地的bat脚本所在路径**重定向为对方服务器的**SMB的特定地址**(//tsclient)。

## 一键执行命令

>  格式：
>
> cmd /k \\tsclient\windows-proxy-init\basicInit.bat **共享主机名** **代理文件下载地址**** ***主机名*** **宽带账号** **宽带密码****
>
> 举例：
>
> cmd /k \\tsclient\windows-proxy-init\basicInit.bat tsclient http://tools.file.irybd.com/windows-proxy-init.rar huzhou06   057247788133  808717

## 特定值说明

以下内容，如果不是前者说明的，那必须修改代理脚本

1. 包含java程序的文件名，是windows-proxy-init.rar
2. download.vbs等文件名不应修改，保持现状



##  运行过程解释

包含在脚本中

## 问题与解决办法说明

1. 运行命令后，提示`\\teclient\....bat`不存在，解决办法：打开文件资源管理器，进入共享文件夹，再重新运行程序。

2. 自动拨号时，如果从excel复制宽带信息可能会丢失账号中开头是0的数字，导致拨号失败。解决办法：把excel的单元格设置成文本格式。

3. 自动拨号时，遇到能拨号成功，但是无法联网，导致死循环。一般是宽带问题,进入了189.cn。解决办法：呼叫客服

4. 自动拨号时，会遇到和账号密码正常，但会出现691问题。解决办法：呼叫客服

5. 自动拨号时，如果遇到初始化时能宽带连接，初始化完毕后宽带失败。解决办法：将宽带连接设置成任何人可连接，而不是只是我。

   

