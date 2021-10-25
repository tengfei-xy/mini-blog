

## 参数

插入到文件开头

```
insertbefore=BOF
```

插入到文件结尾

```
insertbefore=EOF
```

匹配行插入

```
# 匹配行后面
insertafter="^#!/bin/bash"
# 匹配行前面
insertbefore="^#!/bin/bash"
```

指定标记

```
marker="#{mark} this is marker test"'
```

文件备份

```
backup=yes
```

文件是否创建

```
create=yes
```



## 示例

在{{local_repo}}确保特定块内容存在

```yaml
- hosts: ora-ansible
  vars:
    local_repo: /etc/yum.repos.d/local.repo 
  remote_user: root
  tasks:
    - name: file in {{ local_repo }}
      blockinfile:
        create: yes
        path: "{{ local_repo }}"
        block: 
              "[local]\n
              name=local\n
              baseurl=file:///mnt/iso\n
              enabled=1\n
              gpgcheck=0\n"
```

