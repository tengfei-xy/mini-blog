# 利用dism 查看/安装/卸载 windows功能

查看功能状态
dism /online  /Get-Features

安装
Dism /Online /Enable-Feature /FeatureName:MSMQ-Container

卸载
Dism /Online  /disable-Feature /FeatureName:MSMQ-ContainerDisable-Feature

快速安装Telnel Clinet
Dism /Online /Enable-Featurete /FeatureName:TelnetClient

