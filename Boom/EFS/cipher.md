    C:\Users\administrator>cipher /?
    显示或更改 NTFS 分区上目录 [文件] 的加密。
    
      CIPHER [/E | /D | /C]
             [/S:directory] [/B] [/H] [pathname [...]]
    
      CIPHER /K [/ECC:256|384|521]
    
      CIPHER /R:filename [/SMARTCARD] [/ECC:256|384|521]
    
      CIPHER /P:filename.cer
    
      CIPHER /U [/N]
    
      CIPHER /W:directory
    
      CIPHER /X[:efsfile] [filename]
      CIPHER /Y
    
      CIPHER /ADDUSER [/CERTHASH:hash | /CERTFILE:filename | /USER:username]
             [/S:directory] [/B] [/H] [pathname [...]]
    
      CIPHER /FLUSHCACHE [/SERVER:servername]
    
      CIPHER /REMOVEUSER /CERTHASH:hash
             [/S:directory] [/B] [/H] [pathname [...]]
    
      CIPHER /REKEY [pathname [...]]
    /B        如果遇到错误则中止。默认情况下，即使遇到
              错误，CIPHER 也会继续执行。
    /C        显示有关加密文件的信息。
    /D        解密指定的文件或目录。
    /E        加密指定的文件或目录。目录将
              被标记，这样随后添加的文件就会被加密。
              如果父目录未加密，则当修改加密的文件时，
              该文件可能被解密。建议你
              对文件和父目录进行加密。
    /H        显示具有隐藏属性或系统属性的文件。默认
              情况下会忽略这些文件。
    /K        创建新的用于 EFS 的证书和密钥。如果选择
              了此选项，则将忽略所有其他选项。
    
              注意: 在默认情况下，/K 会创建符合当前组策略
                    的证书和密钥。如果指定了 ECC，则会使用
                    提供的密钥大小创建自签名的证书。
    
    /N        此选项只能与 /U 一起使用。它将阻止更新
              密钥。此选项用于查找本地驱动器上的所有
              加密文件。
    /R        生成一个 EFS 恢复密钥和证书，然后将它们写入
              一个 .PFX 文件(包含证书和私钥)和一个
               .CER 文件(只包含证书)。管理员可以向
              EFS 恢复策略添加 .CER 的内容，
              为用户创建恢复密钥并导入 .PFX 以恢复
              各个文件。如果指定了 SMARTCARD，则会将
              恢复密钥和证书写入智能卡。将会
              生成 .CER 文件(仅包含证书)，但不会
              生成 .PFX 文件。
    
              注意: 在默认情况下，/R 会创建 2048 位 RSA 恢复密钥和
                    证书。如果指定了 ECC，则它必须后跟
                    256、384 或 521 大小的密钥。
    
    /P        基于传入的证书创建 base64 编码的恢复
              策略 blob。此 blob 可用于设置用于 MDM 部署
              的 DRA 策略。
    /S        对给定目录以及其中的所有文件和子目录
              执行指定的操作。
    /U        尝试处理本地驱动器上的所有加密文件。如果用户
              的文件加密密钥或恢复密钥已更改，这会将其
              更新为当前的密钥。此选项不能与 /N 以外的其他
              选项一起使用。
    /W        从整个卷上可用的未使用磁盘空间中删除
              数据。如果选择了此选项，则会忽略所有其他选项。
              指定的目录可以是本地卷上的任意位置。如果它
              是装入点或指向另一个卷中的目录，
              则该卷上的数据将被删除。
    /X        将 EFS 证书和密钥备份到 filename 文件中。如果提供
              了 efsfile，将会备份当前用户的用于
              加密此文件的证书。否则，将会备份用户的当前 EFS
               证书和密钥。
    /Y        在本地电脑上显示当前的 EFS 证书缩略图。
    /ADDUSER  向指定的加密文件添加用户。如果提供
              了 CERTHASH，cipher 将搜索带有此 SHA1 哈希的
              证书。如果提供了 CERTFILE，cipher 将从文件
              中提取证书。如果提供了 USER，cipher 将尝试
              在 Active Directory 域服务中查找用户的
              证书。
    /FLUSHCACHE
              清除指定服务器上的调用用户的 EFS 密钥缓存。
              如果未提供 servername，cipher 会清除本地计算机上
              该用户的密钥缓存。
    /REKEY    更新指定的加密文件以使用配置的
              当前 EFS 密钥。
    /REMOVEUSER
              从指定的文件中删除用户。CERTHASH 必须是
              要删除的证书的 SHA1 哈希。
    
    directory 目录路径。
    filename  不带扩展名的文件名。
    pathname  指定一个模式、文件或目录。
    efsfile   加密的文件路径。
    
    如果在不带参数的情况下使用，CIPHER 将显示
    当前目录及其包含的所有文件的加密状态。你可以使用多个目录
    名和通配符。多个参数之间必须有空格。
