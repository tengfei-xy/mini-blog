## 概念：客户端与服务端

**域控制器**使用WinRM连接**域计算机**时，主控是客户端，域计算机是服务端。

**域计算机**使用WinRM连接**域控制器**时，域计算机是客户端，主控是客户端。



## 配置

查看配置

- cmd

  ```bat
  winrm get winrm/config
  ```

- powershell

  ```powershell
  # 查看全部对象
  (Get-PSSessionConfiguration).Name
  
  # 查看全部属性
  Get-PSSessionConfiguration -Name microsoft.powershell.workflow | fl -Property *
  ```

  

快速配置并打开winrm（必须使用管理员权限）

- cmd

  ```bat
  winrm quickconfig
  ```

- powershell

  ```powershell
  Enable-PSRemoting -Force
  ```

  

客户端允许未加密的流量

- cmd（需要管理员，AllowUnencrypted大小写敏感）

  ```bat
  winrm set winrm/config/client @{AllowUnencrypted="true"}
  ```

- powershell

  ```powershell
  Set-item wsman:localhost\client\AllowUnencrypted -value true -Force
  ```

  

客户端信任任何服务端连接（终端：需要管理员，且不要使用win11的wt以及win10的powershell）（TrustedHosts大小写敏感）

- cmd

  ```bat
  winrm set winrm/config/client @{TrustedHosts="*"}
  ```

- powershell

  ```powershell
  Set-item wsman:localhost\client\trustedhosts -value * -Force
  ```

  

设置winrm防火墙规则：允许任何远程主机连接、并允许WinRM规则通过

```powershell
Get-NetFirewallRule -name "*WINRM*" | Set-NetFirewallRule -RemoteAddress any -Enabled true
```

其他参考：关闭防火墙

```powershell
Set-NetFirewallProfile -Enabled false
```

关闭WinRM的被连接/被管理

- 在组策略中WinRM服务项，禁用“允许通过 WinRM 进行远程服务器管理”
- 在组策略中WinRM服务项，允许“允许通过 WinRM 进行远程服务器管理”，但列表为空



# 其他主题

## 一、[关于远程故障排除](https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_remote_troubleshooting)

当另一个域中的用户是本地计算机上 Administrators 组的成员时，该用户不能使用管理员特权远程连接到本地计算机。默认情况下，来自其他域的远程连接只能通过标准用户特权令牌运行。但是，可以使用 LocalAccountTokenFilterPolicy 注册表条目来更改默认行为，允许作为 Administrators 组成员的远程用户通过管理员特权运行。

```powershell
new-itemproperty -path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System -name LocalAccountTokenFilterPolicy -propertyType DWord -value 1 |Out-Null
```



二、[JEA](https://blog.51cto.com/lianggj/1905343)
