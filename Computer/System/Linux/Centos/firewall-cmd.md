# firewall-cmd
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone=public --query-port=80/tcp
删除
firewall-cmd --zone=public --remove-port=80/tcp --permanent
添加
firewall-cmd --zone=public --add-port=80/tcp --permanent