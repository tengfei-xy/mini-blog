# zypper源

查看源

```bash
linux-xuph:~ # zypper lr
Repository priorities are without effect. All enabled repositories share the same priority.
d
# | Alias             | Name              | Enabled | GPG Check | Refresh
--+-------------------+-------------------+---------+-----------+--------
1 | SLES12-SP5-12.5-0 | SLES12-SP5-12.5-0 | Yes     | (r ) Yes  | No     
2 | google-chrome     | google-chrome     | Yes     | ( p) Yes  | Yes    
3 | openSUSE-12.5-Oss | openSUSE-12.5-Oss | Yes     | ( p) Yes  | Yes    
4 | suse              | suse              | Yes     | ( p) Yes  | No 
```

禁用源

```bash
linux-xuph:~ # zypper modifyrepo -d openSUSE-12.5-Oss
Repository 'openSUSE-12.5-Oss' has been successfully disabled.
```

添加源

```纯文本
zypper addrepo -f http://mirrors.aliyun.com/opensuse/distribution/13.1/repo/oss/ openSUSE-13.1-Oss
```

刷新源

```纯文本
zypper refresh
```

安装源

```纯文本
zypper in 软件名
```
