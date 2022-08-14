office的密钥是以二进制的数据类型保存在注册表

```
路径:
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion
项名:
DiagitalProductId
```

但为了解密而获取到密钥，可以使用[ProduKey](https://www.nirsoft.net/utils/product_cd_key_viewer.html#DownloadLinks),双击即可获得

## 通过kms激活产品

获取密钥（最后5位）

```
cscript OSPP.VBS /dstatus | findstr "product.key:"
```



卸载密钥（）

```
cscript OSPP.VBS /unpkey:WFG99
```

