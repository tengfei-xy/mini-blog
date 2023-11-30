# jq

## 目录

-   [数组](#数组)

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

# 数组

获取数组长度

```bash
jq '.Data.list | length'
```

合并数组，作为字符串

```bash
jq -r '.idList | join("")'
```

将字段合并为数组

```bash
data_array=()
id1="86ce210c-ceda-4040-83a7-a6b07679975e"
id2="ecb3ad80-5a04-4b18-a5c1-b2f97c72c9c8"
ans1="A"
ans2="B"
data_array+=("{\"id\":\"${id1}\",\"answer\":\"${ans1}\"}")
data_array+=("{\"id\":\"${id2}\",\"answer\":\"${ans2}\"}")

result=$(echo "${data_array[@]}"  | jq -s .)
echo "$result"

```

从数组中提取值

```bash
#!/bin/bash

data='{"activity_reads": [
        {
            "activity_id": 60002620192,
            "activity_type": "learning_activity",
            "completeness": "part",
            "created_by_id": 60000740050,
            "created_for_id": 60000740050,
            "data": {},
            "id": "65488696047f7460ba65d098",
            "last_visited_at": "2023-11-06T06:36:40.278Z"
        },
        {
            "activity_id": 1,
            "activity_type": "learning_activity",
            "completeness": "full",
            "created_by_id": 2,
            "created_for_id": 3,
            "data": {},
            "id": "4",
            "last_visited_at": "2023-11-06T06:36:40.278Z"
        }]
        }'
echo "$data"  |jq '.activity_reads[] | select(.activity_id == 60002620192).completeness'

```
