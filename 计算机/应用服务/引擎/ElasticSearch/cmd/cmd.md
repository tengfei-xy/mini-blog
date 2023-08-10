# cmd

获取index
curl 192.168.0.35:9292/\_cat/indices?

获取红色index
curl 192.168.0.35:9292/\_cat/indices?v -s | grep ^red

获取节点设置
curl 192.168.0.35:9292/test- \*/\_settings获取节点设置
curl 192.168.0.35:9292/test- \*/\_settings

删除节点
curl -XDELETE 192.168.0.35:9292/test-\*

得到集群中第一个未分配的分片的解释
curl 192.168.0.35:9292/\_cluster/allocation/explain | jq

修改默认分片数和副本
PUT \_template/template1
{
"index\_patterns": \[" \*"],
"settings": {
"number\_of\_shards": 1,
"number\_of\_replicas": 0
}
}修改默认分片数和副本
PUT \_template/template1
{
"index\_patterns": \[" \*"],
"settings": {
"number\_of\_shards": 1,
"number\_of\_replicas": 0
}
}

修改索引refresh值
PUT /logstash-wechatcollectionserver-2019.12.21/\_settings
{ "refresh\_interval": -1 }
{ "refresh\_interval": "1s" }

推迟分片分配
curl -H "Content-Type: application/json" -XPUT '192.168.0.35:9292/logstash-wechatcollectionserver-2019.12.21/\_settings' -d '
{
"settings": {
"index.unassigned.node\_left.delayed\_timeout": "0"
}
}'

获取节点设置
curl -XGET [http://localhost:9200/test-\\](http://localhost:9200/test-\ "http://localhost:9200/test-\\")[http://localhost:9200/test-\\](http://localhost:9200/test-\ "http://localhost:9200/test-\\")[ \*/\_settings](http://localhost:9200/test-\\*/\\_settings " */_settings")[ \*/\_settings](http://localhost:9200/test-\\*/\\_settings " */_settings")

\_cluster/allocation/explain
