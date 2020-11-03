# AD-Control-Golang

## 程序简介

本程序是从AD-Control升级而来,并融合了在AD上经常使用的工具.

##  程序作用简要

### 1. Users.exe - 域用户使用的程序

UI:在DC上以用户权限且任务栏托盘的方式运行
**启动说明：**由组策略的文件替换功能（在开机时），从文件共享中复制到C:\Windows\System32下，并运行。
**具体功能：**

1. 电源选项
2. 软件安装
3. 微盘同步
4. 文件共享



### 2. Domain Computer Daemon - 域计算机使用的Daemon.exe

**运行方式：**由组策略在计算机启动时运行命令runad.bat daemon来进行循环启动DCD。（管理员权限、无窗口 ）
**主要功能：**运行由DCU和ADU提交的程序和与接受处理发送消息（通常处理需要管理员权限的消息）

### 3. ADServer Daemon- 主控使用的Daemon.exe

**运行方式：**前后台任意
**主要作用：**根据Users.exe的参数设定响应来自DC端的请求
**其他：**使用注册表（HKLM\Software\ADControl）作为数据库的存储

### 4. adct.exe 域控制工具

**作用：**通过adct来控制域用户、域计算机来完成部分功能
**具体功能：**

1. 定时关机
2. 汇报情况
3. 强制备份



## 其他说明

### xln/walk的修改

1. 解决托盘菜单中按钮点击一次，会跳出2次的问题

   ```go
   在notifyicon.go的notifyIconWndProc()中添加
   howmenu = !showmenu;!showmenu { return } 和var showmenu bool = false
   ```

   

2. 解决icon文件没有使用指定的绝对路径问题，导致即使指定正确路径也无法启动程序问题。

   ```go
   在func (rm *ResourceManager) Icon(name string) (*Icon, error) {下,
   更新
   if icon, err := NewIconFromFile(filepath.Join(rm.rootDirPath, name)); err == nil {
   为
   if icon, err := NewIconFromFile(name); err == nil {
   ```

   

### 其他问题

1. SlaveAD辅控应启动Daemon,作为备用响应请求程序
2. 托盘中菜单按钮不能实时修改