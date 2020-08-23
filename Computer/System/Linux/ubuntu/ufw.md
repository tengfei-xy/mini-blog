查看状态`ufw status`

启动`ufw enable`

允许端口通过`ufw allow 8080/tcp`

查看规则序号`sudo ufw status numbered`

根据规则序号删除规则`ufw delete 3`

通过端口删除规则`ufw delete allow 8080/tcp`

重置`ufw reset`