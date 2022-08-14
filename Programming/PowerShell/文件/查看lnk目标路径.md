```powershell
# 需要绝对路径且需要.link结束
$lnk='C:\Users\1.lnk'

(New-Object -ComObject WScript.Shell).CreateShortcut($lnk).TargetPath
```

