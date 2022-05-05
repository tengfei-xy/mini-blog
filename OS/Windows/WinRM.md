## 客户端与服务端

**主控**使用WinRM连接**域计算机**时，主控是客户端，域计算机是服务端。

域计算机使用WinRm连接主控时，域计算机是客户端，主控是客户端。

## 关闭被连接/被管理

在组策略中WinRM服务项

- 禁用“允许通过 WinRM 进行远程服务器管理”
- 允许“允许通过 WinRM 进行远程服务器管理”，但列表为空



## 配置

查看配置

```bash
winrm get winrm/config
```

快速配置并打开winrm（必须使用管理员权限）

```
winrm quickconfig
```

客户端允许未加密的流量（终端：需要管理员，且不要使用win11的wt以及win10的powershell）（AllowUnencrypted大小写敏感）

```
winrm set winrm/config/client @{AllowUnencrypted="true"}
```

客户端信任任何服务端连接（终端：需要管理员，且不要使用win11的wt以及win10的powershell）（TrustedHosts大小写敏感）

```
winrm set winrm/config/client @{TrustedHosts="*"}
```



