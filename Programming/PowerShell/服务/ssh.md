查看可安装的openssh

https://www.jianshu.com/p/7837b19366ac

```powershell
PS C:\Users\Administrator>  Get-WindowsCapability -Online -Name OpenSSH*


Name         : OpenSSH.Client~~~~0.0.1.0
State        : Installed
DisplayName  : OpenSSH 客户端
Description  : 基于 OpenSSH 的安全外壳(SSH)客户端，可用于安全密钥管理和远程计算机访问。
DownloadSize : 1323493
InstallSize  : 5301402

Name         : OpenSSH.Server~~~~0.0.1.0
State        : NotPresent
DisplayName  : OpenSSH 服务器
Description  : 基于 OpenSSH 的安全外壳(SSH)服务器，可用于安全密钥管理和远程计算机访问。
DownloadSize : 1297677
InstallSize  : 4946932
```

安装openssh

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```



启动sshd

```powershell
Start-Service sshd
```

设置sshd自动启动

```powershell
Set-Service -Name sshd -StartupType 'Automatic'
```

查看sshd的状态

```powershell
sshd -T | select-string 'subsystem'
subsystem sftp sftp-server.exe
subsystem powershell c:\pwsh\pwsh.exe -sshs -NoLogo -NoProfile
```

