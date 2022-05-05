```powershell
$alias= (Get-NetIPAddress | Where-Object {$_.IPAddress -match "^192"}).InterfaceAlias
set-NetIPAddress -IPAddress 192.168.0.136  -InterfaceAlias $alias
```

