# git更新

当更新ansible版本时,不只要更新git的源码树,也要更新git中指向Ansible自身模块的 “submodules” (不是同一种模块)

```纯文本
git pull --rebase
git submodule update --init --recursive
```
