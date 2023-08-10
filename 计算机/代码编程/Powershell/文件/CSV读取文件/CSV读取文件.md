# CSV读取文件

## 目录

-   [读取CSV文件](#读取CSV文件)
    -   [直接读取](#直接读取)
    -   [读取特定行](#读取特定行)

# 读取CSV文件

## 直接读取

```powershell
foreach($each in Import-Csv $env:USERPROFILE\desktop\情报.csv){
  Write-Host $each[0].hosname
}
```

## 读取特定行

```powershell
$Contents=Import-Csv D:\Servers.csv #将CSV文件中内容导入，,赋值给变量为了利
$csvrows=($Contents | Measure-Object).Count #统计csv中的行数

#用for 循环，逐个读取，CSV和EXCEL 类似，我们取对应的Server，Username，Password 三个栏位的值.
for ($i=0;$i -le ($csvrows-1); $i++ ){
  $Server=$Contents[$i].Server
  $User=$Contents[$i].UserName
  $PWD=$Contents[$i].Password
  Write-Host "The $i line Servername is $Server"
  Write-Host "The $i line Usernane is $User"
  Write-Host"“The $i line Password is $PWD"
}
```
