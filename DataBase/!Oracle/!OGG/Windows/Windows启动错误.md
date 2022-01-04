# Windows server 2012 启动ogg错误



Q：启动时如果报错缺少msvcp140.dll

A：安装[vc++2015 64位](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe)运行库




Q：安装vc++2015报错

A：查看报错日志，可发现安装Windows8.1-KB2999226-x64.msu时出错。

1. 安装KB2919442，文件为：[Windows8.1-KB2999226-x64.msu](https://download.microsoft.com/download/D/6/0/D60ED3E0-93A5-4505-8F6A-8D0A5DA16C8A/Windows8.1-KB2919442-x64.msu)

2. 安装[KB2919355](https://www.microsoft.com/en-us/download/details.aspx?id=42334)，按顺序安装下列文件

   [clearcompressionflag.exe](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/clearcompressionflag.exe) ==注：管理员身份运行，没有界面，后台运行。==

   [Windows8.1-KB2919355-x64.msu](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/Windows8.1-KB2919355-x64.msu) 

   [Windows8.1-KB2932046-x64.msu](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/Windows8.1-KB2932046-x64.msu)

   [Windows8.1-KB2934018-x64.msu](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/Windows8.1-KB2934018-x64.msu)

   [Windows8.1-KB2937592-x64.msu](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/Windows8.1-KB2937592-x64.msu)

   [Windows8.1-KB2938439-x64.msu](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/Windows8.1-KB2938439-x64.msu) 

   [Windows8.1-KB2959977-x64.msu](https://download.microsoft.com/download/2/5/6/256CCCFB-5341-4A8D-A277-8A81B21A1E35/Windows8.1-KB2959977-x64.msu)

3. 安装完成后，需要重启主机，这个安装速度过程根据硬件配置和网络。

