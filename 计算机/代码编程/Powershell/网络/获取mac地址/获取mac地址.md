# 获取mac地址

1.根据ip地址获取mac地址

```powershell
$ip="192.168.0.20"
(Get-NetAdapter -InterfaceIndex (Get-NetIPAddress -IPAddress $ip).InterfaceIndex ).MacAddress
```

2.直接获取

```powershell
PS> getmac /FO CSV

Physical Address                        Transport Name
----------------                        --------------
5C-51-4F-62-F2-7D                       \Device\Tcpip_{FF034A81-CBFE...
5C-51-4F-62-F2-81                       Media disconnected
```
