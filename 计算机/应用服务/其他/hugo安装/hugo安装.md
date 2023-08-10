# hugo安装

<https://gohugo.io/getting-started/installing>

安装依赖

```纯文本
yum -y install git
```

创建主题

```纯文本
hugo new site <主题名>
```

进入主题文件夹

```纯文本
cd xunyang
git init
```

获取主题

```纯文本
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo theme = \"ananke\" >> config.toml
```
