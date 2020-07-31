```shell
#!/bin/bash

echo Getting server IP
node=$(arp | grep ether | grep ^192.168.0.[[:digit:]]* -o | sort)
read -p "Please input password:" password

for ip in $node
do
        # 同步并获取差值
        res=$(sshpass -p $password ssh -t root@$ip "echo $(sudo ntpdate cn.pool.ntp.org | grep 'offset.*' -o)&& (sudo clock -w&)")
        
        #输出报告
        echo $ip\ $res
done
```

