# kms

## 目录

-   [windows、office激活](#windowsoffice激活)
    -   [第一步：在Linux上部署kms服务器](#第一步在Linux上部署kms服务器)
    -   [第二步：激活Windows](#第二步激活Windows)
    -   [其他：激活码](#其他激活码)
        -   [Office 2019](#Office-2019)
        -   [Office 2016](#Office-2016)
        -   [Office 2013](#Office-2013)
        -   [Office 2010](#Office-2010)

# windows、office激活

## 第一步：在Linux上部署kms服务器

clone项目

```纯文本
git clone git@github.com:Wind4/vlmcsd.git
```

指定项目位置

```纯文本
mv vlmcsd-master /usr/local/vlmcsd
```

进入vlmcsd项目路径

```纯文本
cd /usr/local/vlmcsd
```

安装依赖

```纯文本
yum install gcc git make -y
```

编译项目

```纯文本
[root@node21 vlmcsd]# make
make[1]: Entering directory `/usr/local/vlmcsd/src'
	CC	vlmcs.o <- vlmcs.c
	CC	kmsdata-full.o <- kmsdata-full.c
	CC	crypto.o <- crypto.c
	CC	kms.o <- kms.c
	CC	endian.o <- endian.c
	CC	output.o <- output.c
	CC	shared_globals.o <- shared_globals.c
	CC	helpers.o <- helpers.c
	CC	network.o <- network.c
	CC	rpc.o <- rpc.c
	CC	crypto_internal.o <- crypto_internal.c
	CC	dns_srv.o <- dns_srv.c
	CC	vlmcsd.o <- vlmcsd.c
	CC	kmsdata.o <- kmsdata.c
	LD    	../bin/vlmcs <- vlmcs.o kmsdata-full.o crypto.o kms.o endian.o output.o shared_globals.o helpers.o network.o rpc.o crypto_internal.o dns_srv.o
	LD    	../bin/vlmcsd <- vlmcsd.o kmsdata.o crypto.o kms.o endian.o output.o shared_globals.o helpers.o network.o rpc.o crypto_internal.o
make[1]: Leaving directory `/usr/local/vlmcsd/src'
```

启动方式

```纯文本
/usr/local/vlmcsd/bin/vlmcsd -L 0.0.0.0:1688 -l /usr/local/vlmcsd/vlmcsd.log
```

## 第二步：激活Windows

```纯文本
@echo off
slmgr -ipk W269N-WFGWX-YVC9B-4J6C9-T83G
slmgr /skms 192.168.0.21:1688
slmgr /ato
slmgr /xpr
pause
```

## 其他：激活码

### Office 2019

\| Product | GVLK | | —————————– | —————————– | | Office Professional Plus 2019 | NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP | | Office Standard 2019 | 6NWWJ-YQWMR-QKGCB-6TMB3-9D9HK | | Project Professional 2019 | B4NPR-3FKK7-T2MBV-FRQ4W-PKD2B | | Project Standard 2019 | C4F7P-NCP8C-6CQPT-MQHV9-JXD2M | | Visio Professional 2019 | 9BGNQ-K37YR-RQHF2-38RQ3-7VCBB | | Visio Standard 2019 | 7TQNQ-K3YQQ-3PFH7-CCPPM-X4VQ2 | | Access 2019 | 9N9PT-27V4Y-VJ2PD-YXFMF-YTFQT | | Excel 2019 | TMJWT-YYNMB-3BKTF-644FC-RVXBD | | Outlook 2019 | 7HD7K-N4PVK-BHBCQ-YWQRW-XW4VK | | PowerPoint 2019 | RRNCX-C64HY-W2MM7-MCH9G-TJHMQ | | Publisher 2019 | G2KWX-3NW6P-PY93R-JXK2T-C9Y9V | | Skype for Business 2019 | NCJ33-JHBBY-HTK98-MYCV8-HMKHJ | | Word 2019 | PBX3G-NWMT6-Q7XBW-PYJGG-WXD33 |

### Office 2016

| Product                       | GVLK                          |
| ----------------------------- | ----------------------------- |
| Office Professional Plus 2016 | XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99 |
| Office Standard 2016          | JNRGM-WHDWX-FJJG3-K47QV-DRTFM |
| Project Professional 2016     | YG9NW-3K39V-2T3HJ-93F3Q-G83KT |
| Project Standard 2016         | GNFHQ-F6YQM-KQDGJ-327XX-KQBVC |
| Visio Professional 2016       | PD3PC-RHNGV-FXJ29-8JK7D-RJRJK |
| Visio Standard 2016           | 7WHWN-4T7MP-G96JF-G33KR-W8GF4 |
| Access 2016                   | GNH9Y-D2J4T-FJHGG-QRVH7-QPFDW |
| Excel 2016                    | 9C2PK-NWTVB-JMPW8-BFT28-7FTBF |
| OneNote 2016                  | DR92N-9HTF2-97XKM-XW2WJ-XW3J6 |
| Outlook 2016                  | R69KK-NTPKF-7M3Q4-QYBHW-6MT9B |
| PowerPoint 2016               | J7MQP-HNJ4Y-WJ7YM-PFYGF-BY6C6 |
| Publisher 2016                | F47MM-N3XJP-TQXJ9-BP99D-8K837 |
| Skype for Business 2016       | 869NQ-FJ69K-466HW-QYCP2-DDBV6 |
| Word 2016                     | WXY84-JN2Q9-RBCCQ-3Q3J3-3PFJ6 |

### Office 2013

\| Product | GVLK | | —————————– | —————————– | | Office 2013 Professional Plus | YC7DK-G2NP3-2QQC3-J6H88-GVGXT | | Office 2013 Standard | KBKQT-2NMXY-JJWGP-M62JB-92CD4 | | Project 2013 Professional | FN8TT-7WMH6-2D4X9-M337T-2342K | | Project 2013 Standard | 6NTH3-CW976-3G3Y2-JK3TX-8QHTT | | Visio 2013 Professional | C2FG9-N6J68-H8BTJ-BW3QX-RM3B3 | | Visio 2013 Standard | J484Y-4NKBF-W2HMG-DBMJC-PGWR7 | | Access 2013 | NG2JY-H4JBT-HQXYP-78QH9-4JM2D | | Excel 2013 | VGPNG-Y7HQW-9RHP7-TKPV3-BG7GB | | InfoPath 2013 | DKT8B-N7VXH-D963P-Q4PHY-F8894 | | Lync 2013 | 2MG3G-3BNTT-3MFW9-KDQW3-TCK7R | | OneNote 2013 | TGN6P-8MMBC-37P2F-XHXXK-P34VW | | Outlook 2013 | QPN8Q-BJBTJ-334K3-93TGY-2PMBT | | PowerPoint 2013 | 4NT99-8RJFH-Q2VDH-KYG2C-4RD4F | | Publisher 2013 | PN2WF-29XG2-T9HJ7-JQPJR-FCXK4 | | Word 2013 | 6Q7VD-NX8JD-WJ2VH-88V73-4GBJ7 |

### Office 2010

\| Product | GVLK | | —————————– | —————————– | | Office Professional Plus 2010 | VYBBJ-TRJPB-QFQRF-QFT4D-H3GVB | | Office Standard 2010 | V7QKV-4XVVR-XYV4D-F7DFM-8R6BM | | Access 2010 | V7Y44-9T38C-R2VJK-666HK-T7DDX | | Excel 2010 | H62QG-HXVKF-PP4HP-66KMR-CW9BM | | SharePoint Workspace 2010 | QYYW6-QP4CB-MBV6G-HYMCJ-4T3J4 | | InfoPath 2010 | K96W8-67RPQ-62T9Y-J8FQJ-BT37T | | OneNote 2010 | Q4Y4M-RHWJM-PY37F-MTKWH-D3XHX | | Outlook 2010 | 7YDC2-CWM8M-RRTJC-8MDVC-X3DWQ | | PowerPoint 2010 | RC8FX-88JRY-3PF7C-X8P67-P4VTT | | Project Professional 2010 | YGX6F-PGV49-PGW3J-9BTGG-VHKC6 | | Project Standard 2010 | 4HP3K-88W3F-W2K3D-6677X-F9PGB | | Publisher 2010 | BFK7F-9MYHM-V68C7-DRQ66-83YTP | | Word 2010 | HVHB3-C6FV7-KQX9W-YQG79-CRY7T | | Visio Standard 2010 | 767HD-QGMWX-8QTDB-9G3R2-KHFGJ | | Visio Professional 2010 | 7MCW8-VRQVK-G677T-PDJCM-Q8TCP | | Visio Premium 2010 | D9DWC-HPYVV-JGF4P-BTWQB-WX8BJ |
