# cmake

您好！您可以按照以下步骤在CentOS 9上安装CMake：

1.  下载CMake。您可以使用以下命令下载CMake：

```纯文本
wget https://github.com/Kitware/CMake/releases/download/v3.22.0/cmake-3.22.0.tar.gz
```

1.  解压缩下载的文件。您可以使用以下命令解压缩文件：

```纯文本
tar -zxvf cmake-3.22.0.tar.gz
```

1.  进入解压缩后的目录并运行以下命令：

```纯文本
cd cmake-3.22.0
./bootstrap
make
sudo make install
```

这些命令将编译和安装CMake。如果您需要检查已安装的CMake版本，请使用以下命令：

```纯文本
cmake --version
```
