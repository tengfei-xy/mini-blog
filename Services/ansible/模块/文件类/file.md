## 创建文件

```bash
-m file -a "path=/etc/yum.repos.d/local.repo group=root owner=root state=touch"
```

创建链接

```bash
-m file -a "src=/etc/hosts dest=/tmp/hosts state=link"
```



## 复制

本地开始复制

```bash
-m copy -a "src=/etc/hosts dest=/tmp/hosts"
```

远程复制

```bash
 -m copy -a "remote_src=true src=/root/1 dest=/usr/local/ owner=username group=username mode=440"
```



## 创建目录

单个目录

```bash
 -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
```

多个目录

```yaml
- name: 添加文件夹
  file:
    path: "/usr/local/redis-6.2.6/{{item}}"
    state: directory
  with_items:
    - conf
    - data
    - logs
```



## 删除

```bash
 -m file -a "dest=/path/to/c state=absent"
```

