更新系统

```
pacman -Syn
```

安装指定包文件,扩展名为 pkg.tar.gz

```
pacman -U
```



卸载

```
pacman -R 软件名 : 该命令将只删除包，保留其全部已经安装的依赖关系
pacman -Rv 软件名 : 删除软件，并显示详细的信息
pacman -Rs 软件名 : 删除软件，同时删除本机上只有该软件依赖的软件。
pacman -Rsc 软件名 : 删除软件，并删除所有依赖这个软件的程序，慎用
```

查询

```
pacman -Ss 关键字：在仓库中搜索含关键字的包。
pacman -Qs 关键字： 搜索已安装的包。
pacman -Qi 包名：查看有关包的详尽信息。
pacman -Ql 包名：列出该包的文件。
```

只下载包，不安装

```
pacman -Sw
```

清理

- 未安装的包文件，包文件位于 /var/cache/pacman/pkg/ 目录。

  ```
  pacman -Sc
  ```

- 所有的缓存文件

  ```
  pacman -Scc
  ```

  

