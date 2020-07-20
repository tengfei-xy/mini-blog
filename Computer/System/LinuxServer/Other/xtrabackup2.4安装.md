# xtrabackup2.4 安装

下载解压 -> 编译与安装 -> 完成

注:XtraBackup 需要和 Mysql 的版本匹配
以下是 XtraBackup2.4,其中 cmake版本为2.8,并需要boost1.59

一、下载并解压xtrabackup-2.4.14
$ wget https://www.percona.com/downloads/Percona-XtraBackup-2.4/Percona-XtraBackup-2.4.14/source/tarball/percona-xtrabackup-2.4.14.tar.gz
$ tar zxvf percona-xtrabackup-2.4.14.tar.gz
$ cd percona-xtrabackup-2.4.14

二、cmake 编译选项
注: 根据-DWITH\_BOOST 参数,如果路径之下的库不存在,将会自动下载
$ cmake . -DCMAKE\_INSTALL\_PREFIX=/usr/local/xtrabackup -DBUILD\_CONFIG=xtrabackup\_release -DWITH\_MAN\_PAGES=OFF -DWITH\_BOOST=/usr/local/boost\_1\_59\_0
$ make install 

三、完成
安装路径:/usr/local/xtrabackup