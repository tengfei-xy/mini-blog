```
@echo off
cls
SETLOCAL ENABLEDELAYEDEXPANSION
rem 检查参数是否满足4个
if %4 EQU "" (
echo parameter error
goto batexit 
)

echo -----  S/Get Basic Info  -----
set pppoename=宽带连接
for /F %%a in ('cmd /c reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" ') do (
SET b=%%a
SET j=!b:~0,4!
if "!j!" NEQ "Defa" (
if "!j!" NEQ "Save" (
set regaccount=%%a
)
)
)
if "%regaccount%" NEQ ""  (
if "%regaccount:~0,4%" NEQ "HKEY" (
set pppoename=!regaccount!
)
)

rem 变量设置
set smbfolder=%1
set downloadlink=%2
set proxyhostname=%3
set account=%4
set password=%5
set adminpass=zhiweidata
set proxyfile=windows-proxy-init
set proxyfileext=%proxyfile%.rar
set desktop=%userprofile%\desktop
set deskclientfolder=%desktop%\client
set deskinitfolder=%desktop%\%proxyfile%

rem 变量呼出
echo redirect folder: %smbfolder%
echo Download Link: %downloadlink%
echo Default Proxy File: %proxyfileext%
echo Proxy Hostname: %proxyhostname%
echo PPPOE Name: %pppoename%
echo PPPOE Account: %account%
echo PPPOE Password: %password%

echo.
echo ------  PPPOE  ------
:PppoeConnect
echo 断开 %pppoename%
rasdial %pppoename% /disconnect
echo 连接  %pppoename%
rasdial %pppoename% %account% %password%

rem 如果连接pppoe失败
ping 1.2.4.8 -n 2 | findstr TTL >nul
if (%errorlevel% EQU 1) (
goto PppoeConnect
)

rem 如果已经下载文件
if EXIST "%desktop%\%proxyfileext%" (
goto rar
)

rem 如果映射驱动器存在 %proxyfileext% ，否则下载
echo.
if EXIST "\\%smbfolder%\%proxyfile%\%proxyfileext%" (
echo -- copy file from \\%smbfolder%\%proxyfile%\%proxyfileext% --
copy \\%smbfolder%\%proxyfile%\%proxyfileext% %desktop% /y
) else (
echo -- downloading file from %downloadlink% --
cd %desktop%
echo.
\\%smbfolder%\%proxyfile%\wget.exe %downloadlink% -O %desktop%\%proxyfileext%
)

:rar
echo.
echo -----  unrar file %proxyfileext% -----
rem 使用本地rar命令程序
if EXIST "C:\Program Files\WinRAR\Rar.exe" (
echo use local rar.exe
"C:\Program Files\WinRAR\Rar.exe" x -t -o-p %desktop%\%proxyfileext% %desktop%
rem 使用远程rar命令程序
) else (
echo use remote rar.exe
\\%smbfolder%\%proxyfile%\rar.exe x -t -o-p %desktop%\%proxyfileext% %desktop%
)

if NOT EXIST "%desktop%\%proxyfile%" (
echo rar file is error.
goto batexit
)

echo.
echo -----  ready init client -----

echo move client to desktop
move /Y %deskinitfolder%\client %desktop%\

echo move javato ProgramFile
move /Y %deskinitfolder%\Java "%ProgramFiles%"

echo init group poilcy
regedit /s %deskinitfolder%\init_grouppoilcy.reg

echo init java system path
regedit /s %deskinitfolder%\init_javapath.reg

echo init tcp set
regedit /s %deskinitfolder%\init_tcp.reg

echo init admin$ set
regedit /s %deskinitfolder%\init_admin_share.reg

echo move new bat to client folder
move /Y %deskinitfolder%\*.bat %deskclientfolder%

echo \\%smbfolder%\%proxyfile%覆盖run.bat
copy /Y \\%smbfolder%\%proxyfile%\run.bat %deskclientfolder%

echo \\%smbfolder%\%proxyfile%覆盖kill_start.bat
copy /Y \\%smbfolder%\%proxyfile%\kill_start.bat %deskclientfolder%

rem 手动生成start.bat
echo start.bat:
echo C:\Users\Administrator\Desktop\client\run.bat windows-%proxyhostname% %pppoename% %account% %password%
echo @C:\Users\Administrator\Desktop\client\run.bat windows-%proxyhostname% %pppoename% %account% %password% >>%deskclientfolder%\start.bat

if NOT EXIST "%deskclientfolder%\start.bat" (
echo ERROR: start.bat is not exist
)




echo.
echo -----  init will over  -----
rmdir /q /s %deskinitfolder%
erase %desktop%\%proxyfileext%
echo mod adminsitrator'password to %adminpass%
net user Administrator %adminpass%
rem 关闭防火墙
sc config mpssvc start= disabled
sc config LanmanServer start= auto
echo system will reboot at 0s later
shutdown -r -t 0
msg * everthing is ok!
exit /b 0
:batexit
exit /b 1
```

