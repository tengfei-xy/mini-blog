# .rdp

## 目录

-   [免密码远程](#免密码远程)

# 免密码远程

1.  生成密码,注：生成的密码可能超过一行能显示的400个字符，因此需要重定向
    ```powershell
     $Password="xxx"

    [System.Text.UnicodeEncoding]$encoding = [System.Text.Encoding]::Unicode
    [byte[]]$passwordAsBytes = $encoding.GetBytes($Password)
    [byte[]]$passwordEncryptedAsBytes = [System.Security.Cryptography.ProtectedData]::Protect($passwordAsBytes, $null, [System.Security.Cryptography.DataProtectionScope]::CurrentUser)
    [string]$passwordEncryptedAsHex = -join ($passwordEncryptedAsBytes | ForEach-Object { $_.ToString("X2") }) 

    echo [string]$passwordEncryptedAsHex > $env:userprofile\desktop\rdp_password.txt
    ```
2.  在rdp文件中，username行下添加
    ```纯文本
    password 51:b:加密的密码
    ```
3.  在rdp文件中，修改promptcredentialonce行
    ```纯文本
    promptcredentialonce:i:1
    ```
