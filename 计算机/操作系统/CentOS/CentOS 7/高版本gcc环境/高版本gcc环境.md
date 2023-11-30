# 高版本gcc环境

安装依赖(执行2次)

```纯文本
sudo yum -y install centos-release-scl devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils scl-utils gcc automake autoconf libtool make
```

scl方式进入版本的高版本的gcc环境

```bash
# 如果gcc小于5.3版本，临时有效，退出shell会恢复原 gcc 版本
scl enable devtoolset-9 bash

# 其他方式
# https://zhuanlan.zhihu.com/p/382390995
# sudo yum install devtoolset-9-gcc-c++
# source /opt/rh/devtoolset-9/enable
```
