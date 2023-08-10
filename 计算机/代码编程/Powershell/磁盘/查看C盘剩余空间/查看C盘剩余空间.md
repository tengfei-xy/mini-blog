# 查看C盘剩余空间

查看C盘占用

```纯文本
(Get-Volume | Select-Object -Property DriveLetter,SizeRemaining | Where-Object {$_.DriveLetter -eq "C"}).SizeRemaining / 1GB
```
