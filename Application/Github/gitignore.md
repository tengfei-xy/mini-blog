## 格式说明

| 格式         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| bin/         | 忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件 |
| /bin         | 忽略根目录下的bin文件                                        |
| /*.c         | 忽略 cat.c，不忽略 build/cat.c                               |
| debug/*.obj  | 忽略 debug/io.obj，不忽略 debug/common/io.obj 和 tools/debug/io.obj |
| **/foo       | 忽略/foo, a/foo, a/b/foo等                                   |
| a/**/b       | 忽略a/b, a/x/b, a/x/y/b等                                    |
| !/bin/run.sh | 不忽略 bin 目录下的 run.sh 文件                              |
| *.log        | 忽略所有 .log 文件                                           |
| config.php   | 忽略当前路径的 config.php 文件                               |

## 注意事项

- git 对于 .gitignore配置文件是按行从上到下进行规则匹配的

- 注意：如果你创建.gitignore文件之前就push了某一文件，那么即使你在.gitignore文件中写入过滤该文件的规则，该规则也不会起作用，git仍然会对该文件进行版本管理。此时需要清理缓存再提交

```
git rm -r --cached .
```

## 常用配置

```
**/.DS_Store
.vscode/
```

