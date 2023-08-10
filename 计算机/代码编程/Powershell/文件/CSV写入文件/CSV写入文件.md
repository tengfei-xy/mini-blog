# CSV写入文件

## 目录

-   [写入CSV](#写入CSV)
    -   [Export-Csv](#Export-Csv)
    -   [参数说明](#参数说明)

# 写入CSV

## Export-Csv

官方文档：[expoer-csv](https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6 "expoer-csv")

注：需要使用New-Object 建立一个InputObject

## 参数说明

-Append：追加内容

-NoTypeInformation：从输出中移除 # type 信息头。 此参数成为 PowerShell 6.0中的默认参数，并包含在向后兼容性中。

```纯文本
$file="$env:USERPROFILE\desktop\获取开机情况.csv"
Remove-Item $file
foreach ($pc in Get-ChildItem HKLM:\SOFTWARE\ADControl\DomainComputer -Name){
$pcline=Get-ItemProperty HKLM:\SOFTWARE\ADControl\DomainComputer\$pc

$line=@{"hostname"=$pc;"LoginTime"=$pcline.loginTime;"LoginUser"=$pcline.loginUser;};
Export-Csv -Append $file -NoTypeInformation -Force -InputObject (New-Object -TypeName PSObject -Prop $line)
}
```
