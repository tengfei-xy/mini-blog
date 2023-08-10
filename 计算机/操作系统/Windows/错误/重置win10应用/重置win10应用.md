# 重置win10应用

重新安装win10应用，但依赖**AppXSVC**服务

```纯文本
Get-AppXPackage -AllUsers | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
```
