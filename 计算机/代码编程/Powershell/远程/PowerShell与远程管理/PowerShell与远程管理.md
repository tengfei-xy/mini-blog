# PowerShell与远程管理

## 目录

-   [PowerShell](#PowerShell)
    -   [版本](#版本)
    -   [文件格式](#文件格式)
    -   [常用命令](#常用命令)
        -   [Get-Member](#Get-Member)
        -   [Get-Help](#Get-Help)
        -   [Select-Object](#Select-Object)
        -   [Where-Object](#Where-Object)
        -   [Get-history](#Get-history)
    -   [其他](#其他)
-   [一、远程主机进行PowerShell/cmd](#一远程主机进行PowerShellcmd)
    -   [主要功臣：WinRM](#主要功臣WinRM)
    -   [配置](#配置)
    -   [条件](#条件)
    -   [测试](#测试)
    -   [注](#注)
    -   [例](#例)
        -   [批量主机 远程执行 批量PS命令](#批量主机-远程执行-批量PS命令)
        -   [文件远程下载/上传](#文件远程下载上传)
    -   [MAC或Linux 使用PowerShell](#MAC或Linux-使用PowerShell)
-   [二、mstsc](#二mstsc)
    -   [命令行参数](#命令行参数)
    -   [RDP文件](#RDP文件)
    -   [例子](#例子)
    -   [MAC和ipad远程windows](#MAC和ipad远程windows)
    -   [其他](#其他)
-   [三、Sysinternals 工具集](#三Sysinternals-工具集)
    -   [注](#注)
    -   [工具举例](#工具举例)
        -   [远程前台运行程序](#远程前台运行程序)
        -   [查看远程主机运行的程序](#查看远程主机运行的程序)
-   [四、运维神器 -> Windows Admin Center](#四运维神器---Windows-Admin-Center)
    -   [基本功能](#基本功能)

## PowerShell

上得了Linux，下得了cmd，爬得了虫，处理得了Excel

### 版本

查看当前PowerShell版本：

```powershell
PS > Write-Host $PSVersionTable.PSVersion
```

windows 10 默认安装PowerShell5.0.1版本;win 7 默认安装PowerShell 2.0

### 文件格式

PS1：脚本文件(如同cmd的批处理的bat文件)

PSM1：模块文件（模块名需要和文件名相同）

PSD1：模块描述文件

### 常用命令

#### Get-Member

说明：获取成员属性和方法；

缩写：gm

#### Get-Help

说明：命令帮助

缩写：man

#### Select-Object

说明：选择对象或对象属性

缩写：select

#### Where-Object

说明：条件过滤

缩写：where

#### Get-history

说明：查看历史记录

缩写：h

### 其他

继承脚本环境：windows 10 PowerShell ISE(只支持到5.0.1)；更高版本使用VS Code插件

## 一、远程主机进行PowerShell/cmd

### 主要功臣：WinRM

cmd 利用winrm命令进行远程管理和配置

PowerShell 利用[Enter-Pssession](https://docs.microsoft.com/zh-tw/powershell/module/microsoft.powershell.core/enter-pssession?view=powershell-7 "Enter-Pssession")进行远程主机管理
进行远程的主机是客户端，被远程主机是服务端。

### 配置

设置：组策略-计算机配置-管理模板-WIndows组件-Windows远程管理

查看：`PS > winrm get winrm/config`

### 条件

-   双方需要协调并启动对应的身份严重或设置信任主机
-   允许防火墙通过（在建立防火墙规则中，选择入站规则-预定义）
-   WinRM服务运行中

### 测试

`PS > Test-WSMan RemoteIP`

### 注

-   部分命令 需要加上`|`管道符以及`out-string` 才能正常返回运行结果
-   `Enter-PSSession localhost`是不需要身份协商

### 例

#### 批量主机 远程执行 批量PS命令

<https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-6>

`PS > (Get-Content $env:userprofile\desktop\ip.txt) -FilePath C:\Scripts\Sample.ps1 -ArgumentList Process, Service`
说明：获取ip.txt内容中主机，（总是后台）执行Sample.ps1文件

#### 文件远程下载/上传

从 ADServer 的 F:/logs/daemon.log 下载到本地的当前目录

```powershell
$name=Enter-PSSession ADServer
Copy-Itm -Path F:/logs/daemon.log -Destination ./ -FromSession $name
```

从 ADServer 的 F:/logs/daemon.log 下载到本地的当前目录

```powershell
$name=Enter-PSSession ADServer
Copy-Itm -Path D:\PS\Each-PC.ps1 -Destination F:\ -ToSession $name
```

### MAC或Linux 使用PowerShell

<https://github.com/PowerShell/PowerShell>

## 二、mstsc

mstsc(Microsoft terminal services client)

### 命令行参数

/v ***RemoteIP*** 指定远程ip
/shadow:***SessionID***
/control 控制会话

### RDP文件

保留配置参数，包括重定向共享目录，

### 例子

mstsc /v 192.168.0.1 远程桌面
mstsc /v 192.168.0.1 /shadow:1 俗称监控桌面
mstsc /v 192.168.0.1 /shadow:1 /control 俗称远程协助

### MAC和ipad远程windows

MAC：Microsoft Remote Desktop
ipad：RD Client
这俩款软件都只能建立新会话。MAC能利用聚焦搜索到主机，从而直接远程

### 其他

默认情况下
无论是建立新会话还是指定会话时，如果已有用户登录，将会提示对方。
想要取消提醒的话，需要从组策略设置并加入`/noConsentPrompt`参数

## 三、Sysinternals 工具集

说明：是一种以命令行的形式远程管理计算机的工具集合
工具总览：[链接](https://docs.microsoft.com/en-us/sysinternals/downloads/ "链接")

### 注

1.  部分命令需要开启Admin\$
2.  工具集需要手动允许协议，才能正常运行，或加入`-accept`参数

### 工具举例

#### 远程前台运行程序

方法：利用[psLoggedOn](https://docs.microsoft.com/en-us/sysinternals/downloads/psloggedon "psLoggedOn")先查看Session ID,再利用[psExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec "psExec")执行程序
查询到对方主机登录的Session ID 为2
`psexec \\tech111-pc -s -d -i 2 notepad.exe`
说明：在tech111-pc的2id上运行notepad.exe并直接退出

#### 查看远程主机运行的程序

[psList](https://docs.microsoft.com/en-us/sysinternals/downloads/pslist "psList")`pslist.exe \\jd142-pc lantern`
说明：在jd142-pc上查看lantern进程
注：不包含格式后缀

## 四、运维神器 -> Windows Admin Center

[下载链接](https://www.microsoft.com/zh-CN/evalcenter/evaluate-windows-admin-center "下载链接")
运行前提条件：需要正常运行WinRM服务
官方明文规定：该运维神器不能运行于域控制器中。

### 基本功能

-   管理本地用户和组
-   管理磁盘
-   管理防火墙
-   管理服务
-   管理任务计划
-   管理进程（查看、结束）
-   管理设备
-   查看事件
-   管理网卡（查看网卡和修改ip配置）
-   管理文件（查看和上传文件、设置共享等）
-   管理应用和功能（卸载软件、添加可选功能）
-   远程桌面
-   查看证书
-   管理注册表
-   性能监视器、远程PowerShell等
