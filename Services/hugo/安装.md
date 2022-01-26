[安装文档](https://gohugo.io/getting-started/installing)

安装依赖

```
yum -y install git
```



创建主题

```
hugo new site <主题名>
```

进入主题文件夹

```
cd xunyang
git init
```

获取主题

```
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo theme = \"ananke\" >> config.toml
```

