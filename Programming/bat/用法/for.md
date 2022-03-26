n变量累加

```
setlocal ENABLEDELAYEDEXPANSION
set /A n=0
set md5=048d21a406bcff11a6d222908ed6be00
for /F %%i in ('C:\Windows\System32\certutil.exe -hashFile C:\Users\Administrator\Desktop\SSTap.exe MD5 ') do (
set /A n+=1
echo !n!
)
```

delims的分隔符为双引号,表达式

```
for /F tokens^=2^,4^ delims^=^"  %%b in ( 'type C:\Users\Administrator\Desktop\client_update_list.json.save^| findstr .jar' ) do (
	echo %%b %%c
	echo.
)

```

cmd /k \\tsclient\windows-proxy-init\basicInit.bat windows-chengdu01
