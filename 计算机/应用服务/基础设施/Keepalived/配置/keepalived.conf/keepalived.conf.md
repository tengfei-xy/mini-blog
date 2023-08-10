# keepalived.conf

全局配置定义

```bash
# 全局定义标识
global_defs {
    # 通知收件人地址
    notification_email {
        email
        email
    }
    # 通知发送邮件地址
    notification_email_from email
    # smtp服务器地址
    smtp_server host
    # smtp服务器连接超时时间
    smtp_connect_timeout num
    # 指定LVS导向器的名字
    lvs_id string
}
```

虚拟路由定义

```bash
vrrp_instance VI_1 {        #定义实例
    state MASTER            #指定keepalived节点的初始状态，可选值为MASTER|BACKUP
    interface eth0          #VRRP实例绑定的网卡接口，用户发送VRRP包
    virtual_router_id 51    #虚拟路由的ID，同一集群要一致
    priority 100            #定义优先级，按优先级来决定主备角色，优先级越大越优先
    nopreempt               #设置不抢占
    advert_int 1            #主备通讯时间间隔
    authentication {        #配置认证
        auth_type PASS      #认证方式，此处为密码
        auth_pass 1111      #同一集群中的keepalived配置里的此处必须一致，推荐使用8位随机数
    }
    virtual_ipaddress {     #配置要使用的VIP地址
        192.168.200.16
    }
}
```

虚拟服务器定义

```bash
virtual_server 10.10.10.2 1358 {    #配置虚拟服务器
    delay_loop 6        #健康检查的时间间隔
    lb_algo rr          #lvs调度算法
    lb_kind NAT         #lvs模式
    persistence_timeout 50      #持久化超时时间，单位是秒
    protocol TCP        #4层协议

    sorry_server 192.168.200.200 1358   #定义备用服务器，当所有RS都故障时用sorry_server来响应客户端

    real_server 192.168.200.2 1358 {    #定义真实处理请求的服务器
        weight 1                        #给服务器指定权重，默认为1
        HTTP_GET {
            url {
              path /testurl/test.jsp    #指定要检查的URL路径
              digest 640205b7b0fc66c1ea91c463fac6334d   #摘要信息
            }
            url {
              path /testurl2/test.jsp
              digest 640205b7b0fc66c1ea91c463fac6334d
            }
            url {
              path /testurl3/test.jsp
              digest 640205b7b0fc66c1ea91c463fac6334d
            }
            connect_timeout 3       #连接超时时间
            nb_get_retry 3          #get尝试次数
            delay_before_retry 3    #在尝试之前延迟多长时间
        }
}
```

部分配置说明
vrrp\_instance段配置

```bash
nopreempt      #设置为不抢占。默认是抢占的，当高优先级的机器恢复后，会抢占低优先 
               #级的机器成为MASTER，而不抢占，则允许低优先级的机器继续成为MASTER，即使高优先级 
               #的机器已经上线。如果要使用这个功能，则初始化状态必须为BACKUP。

preempt_delay  #设置抢占延迟。单位是秒，范围是0---1000，默认是0.发现低优先 级的MASTER后多少秒开始抢占。
```

vrrp\_script段配置

```bash
#作用：添加一个周期性执行的脚本。脚本的退出状态码会被调用它的所有的VRRP Instance记录。
#注意：至少有一个VRRP实例调用它并且优先级不能为0.优先级范围是1-254.
vrrp_script <SCRIPT_NAME> {
          ...
    }

#选项说明：
script "/path/to/somewhere"     #指定要执行的脚本的路径。
interval <INTEGER>              #指定脚本执行的间隔。单位是秒。默认为1s。
timeout <INTEGER>               #指定在多少秒后，脚本被认为执行失败。
weight <-254 --- 254>           #调整优先级。默认为2.
rise <INTEGER>                  #执行成功多少次才认为是成功。
fall <INTEGER>                  #执行失败多少次才认为失败。
user <USERNAME> [GROUPNAME]     #运行脚本的用户和组。
init_fail                       #假设脚本初始状态是失败状态。

#weight说明： 
#1. 如果脚本执行成功(退出状态码为0)，weight大于0，则priority增加。
#2. 如果脚本执行失败(退出状态码为非0)，weight小于0，则priority减少。
#3. 其他情况下，priority不变。
```

real\_server段配置

```bash
weight <INT>            #给服务器指定权重。默认是1
inhibit_on_failure      #当服务器健康检查失败时，将其weight设置为0， 
                        #而不是从Virtual Server中移除
notify_up <STRING>      #当服务器健康检查成功时，执行的脚本
notify_down <STRING>    #当服务器健康检查失败时，执行的脚本
uthreshold <INT>        #到这台服务器的最大连接数
lthreshold <INT>        #到这台服务器的最小连接数
```

tcp\_check段配置

```ruby
connect_ip <IP ADDRESS>     #连接的IP地址。默认是real server的ip地址
connect_port <PORT>         #连接的端口。默认是real server的端口
bindto <IP ADDRESS>         #发起连接的接口的地址。
bind_port <PORT>            #发起连接的源端口。
connect_timeout <INT>       #连接超时时间。默认是5s。
fwmark <INTEGER>            #使用fwmark对所有出去的检查数据包进行标记。
warmup <INT>                 #指定一个随机延迟，最大为N秒。可防止网络阻塞。如果为0，则关闭该功能。
retry <INIT>                #重试次数。默认是1次。
delay_before_retry <INT>    #默认是1秒。在重试之前延迟多少秒。
```

作者：欧耶90
链接：[https://www.jianshu.com/p/a910e91d43a3](https://www.jianshu.com/p/a910e91d43a3 "https://www.jianshu.com/p/a910e91d43a3")
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
