执行后台任务，-B：最长时间，-P：0为不获取信息，60为每隔60秒获取一次信息

```
ansible all -B 3600 -P 0 -a "/usr/bin/long_running_operation --do-stuff"
```



查看运行状态

```
ansible web1.example.com -m async_status -a "jid=488359678239.2844"
```

