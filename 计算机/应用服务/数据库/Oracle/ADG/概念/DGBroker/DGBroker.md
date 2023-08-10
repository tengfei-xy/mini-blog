# DGBroker

在配置完成Data Guard后，若需要实现主备数据库间的切换，需要在主数据库及备用数据库上分别输入多个命令，切换步骤稍显麻烦。所以，一般情况下，DBA会将整个切换过程编辑成脚本，以便自动运行，进行状态切换。oracle 提供了工具Data Guard Broker，仅在控制端输入一个命令就能方便实现主备数据库间的切换。

Data Guard Broker是建立在Data Guard基础上的一个对Data Guard配置，集中管理操作的一个平台。Broker的推出是为了简化DG复杂的管理过程，它最大的作用就是集中化的统一管理。配置Data Guard Broker使用到的客户端工具是DGMGRL。它是一个命令行管理工具。

在Data Guard Broker的基础上，配置并启用Fast-Start Failover，就能自动检测发现主机故障，实现主备切换，故障转移。

<https://www.huaweicloud.com/articles/3b97db355dcb4dfefffa45f879b9b4ee.html>
