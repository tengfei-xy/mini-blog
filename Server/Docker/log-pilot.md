# log-pilot

启动日志采集到 logstash --rm
sudo docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock -v /etc/localtime:/etc/localtime -v /:/host:ro --cap-add SYS\_ADMIN  --env "LOGGING\_OUTPUT=logstash" --env "PILOT\_TYPE=filebeat" --env "LOGSTASH\_HOST=192.168.0.35" --name logpilot --privileged --env "LOGSTASH\_PORT=5050"  log-pilot:latest

启动日志采集到 logstash --restart=always
sudo docker run -d -v /var/run/docker.sock:/var/run/docker.sock -v /etc/localtime:/etc/localtime -v /:/host:ro --restart=always --cap-add SYS\_ADMIN  --env "LOGGING\_OUTPUT=logstash" --env "PILOT\_TYPE=filebeat" --env "LOGSTASH\_HOST=192.168.0.35" --name logpilot --privileged --env "LOGSTASH\_PORT=5050"  harbor.docker.zhiweireach.com/service/log-pilot:latest

## 标准输出
sudo docker run --rm -it -v /etc/localtime:/etc/localtime --name logpilottest -v /usr/local/logpilot-6.1:/root --label aliyun.logs.logtest=stdout python:latest python /root/to.sh

## 文件输出
sudo docker run --rm -it -v /etc/localtime:/etc/localtime --name timeo -v /home/tengfei:/root --label aliyun.logs.logolo=/root/logs/output.txt python:latest python /root/to.py


sudo docker run --rm -it -v /etc/localtime:/etc/localtime --name timeo -v /home/tengfei:/root --label aliyun.logs.logolo=/root/logs/output.txt --label aliyun.logs.logolo=stdout python:latest python /root/to.py