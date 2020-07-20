# 查看windows 可选功能
查看功能说明(1表示安装,2表示没有安装)
Get-WmiObject -Class Win32\_OptionalFeature |Select Name,Caption,InstallState

