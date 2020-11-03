```bash
#!/bin/bash
node=$(arp | grep ether | grep ^192.168.0.[[:digit:]]* -o | sort)

for var in $node
do
	serv_timestamp=$(ssh $var "date \"+%s\"")
	this_timestamp=$(date "+%s")
	gap_sec=$[this_timestamp-serv_timestamp]

	#如果是负数，计算绝对值
	if [ $gap_sec -lt 0 ]; then
		abs_gap_sec=$[0-gap_sec]
	else
		abs_gap_sec=$gap_sec
	fi

	#输出报告
	echo now_time\ $(date -d  @$this_timestamp "+%H:%M:%S")\ \ $var\ $(date -d  @$serv_timestamp "+%H:%M:%S")\ \ $gap_sec

	#如果超过2秒
	if [ $abs_gap_sec -ge 2 ];then
		#同步时间
		serv_update=$(ssh $var "sudo ntpdate cn.pool.ntp.org && sudo clock -w")
	fi
	
done
```

