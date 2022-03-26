单个ip 执行命令

```powershell
$iplist = Get-Content C:\Users\tengfei\Desktop\ip.txt

foreach ($ip in $iplist){
Write-Host $ip
Invoke-Command -ComputerName $ip -ScriptBlock { }
   pause
} 
```

批量ip执行命令

```powershell
Invoke-Command -ComputerName (Get-Content C:\Users\tengfei\Desktop\ip.txt) -ScriptBlock { (write-Host $env:computername) -add  ()}


Invoke-Command -ComputerName (Get-Content C:\Users\tengfei\Desktop\ip.txt) -ScriptBlock {Get-Volume  -add  write-Host $env:computername}
```

执行本地脚本

```powershell
Invoke-Command -ComputerName tech135-pc -filepath ./test.ps1
```

执行本地脚本附加参数

```powershell
Invoke-Command -ComputerName tech135-pc -filepath ./test.ps1 -ArgumentList 233
```

