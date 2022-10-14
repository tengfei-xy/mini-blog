安装jq命令

```
wget https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -O /usr/local/bin/jq && chmod +x /usr/local/bin/jq
```

访问方式（大小写敏感）：

```
jq '.Data[4].ChildNodeList[3].LookTime'
```

