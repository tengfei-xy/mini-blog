# yum失败

## 目录

-   [yum 失败](#yum-失败)

# yum 失败

Q: Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
S1: yum --disablerepo=epel -→ update ca-certificates
S2: 修改/etc/yum.repos.d/epel.repo文件,取消 baseurl 的注释,并注释 mirrorlist 行
