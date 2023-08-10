# jq

[手册](https://stedolan.github.io/jq/manual/v1.6/ "手册")

安装jq命令

```bash
wget https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -O /usr/local/bin/jq && chmod +x /usr/local/bin/jq
```

访问方式（大小写敏感）：

```bash
jq '.Data[4].ChildNodeList[3].LookTime'
```

输出为字符串，而非格式化的JSON

```bash
jq '. | tostring'
```

多个对象合并

```bash
jq -s add
```
