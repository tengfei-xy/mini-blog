```powershell
$lnk='C:\users\Public\Desktop\Google Chrome.lnk'
(New-Object -ComObject WScript.Shell).CreateShortcut($lnk).TargetPath
```

