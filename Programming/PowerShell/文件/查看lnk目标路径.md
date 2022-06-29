```powershell
# 需要绝对路径且需要.link结束
$lnk='C:\233.lnk'

$lnk='C:\users\linyao\recent\巨量组测试卷B202206 (2).lnk'
(New-Object -ComObject WScript.Shell).CreateShortcut($lnk).TargetPath
```

