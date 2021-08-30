## 创建文件

```
 ansible atlanta -m file -a "path=/etc/yum.repos.d/local.repo group=root owner=root state=touch"
```

创建链接

```
 ansible atlanta -m file -a "src=/etc/hosts dest=/tmp/hosts state=link"
```



## 复制

简单复制命令(通常是从本地开始复制)

```bash
 ansible atlanta -m copy -a "src=/etc/hosts dest=/tmp/hosts"
```



## 创建目录

```bash
ansible webservers -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
```



## 删除

```bash
ansible webservers -m file -a "dest=/path/to/c state=absent"
```

