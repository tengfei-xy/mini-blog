# srvctl管理

## 目录

-   [使用SRVCTL启动和关闭](#使用SRVCTL启动和关闭)
-   [dministering Oracle ASM Instances with SRVCTL in Oracle RAC](#dministering-Oracle-ASM-Instances-with-SRVCTL-in-Oracle-RAC)

# 使用SRVCTL启动和关闭

<https://docs.oracle.com/cd/E11882_01/rac.112/e41960/admin.htm#RACAD811>

本节假设您正在为数据库使用SPFILE。

从命令行输入以下SRVCTL语法，提供所需的数据库名称和实例名称，或包含多个实例名称以启动多个特定实例：

要启动管理员管理的数据库，请输入逗号分隔的实例名称列表：

```纯文本
$ srvctl start instance -d db_unique_name -i instance_name_list [-o start_options]
```

要启动策略管理的数据库，请输入单个节点名称：

```纯文本
$ srvctl start instance -d db_unique_name -n node_name [-o start_options]
```

Note that this command also starts all enabled and non-running services that have `AUTOMATIC` management policy, and for which the database role matches one of the service's roles.

要停止一个或多个实例，请从命令行输入以下SRVCTL语法：

```纯文本
$ srvctl stop instance -d db_unique_name [ -i "instance_name_list" |-n node_name ] [ -o stop_options ]
```

您可以输入逗号分隔的实例名列表以停止多个实例，也可以输入节点名称以停止一个实例。在Windows中，您必须以双引号（“”）括起逗号分隔的列表。

This command also stops the services related to the terminated instances on the nodes where the instances were running. As an example, the following command shuts down the two instances, `orcl3` and `orcl4`, on the `orcl` database using the `immediate` stop option:

```纯文本
$ srvctl 停止实例 -d orcl -i“orcl3,orcl4” -o 立即
```

要启动或停止整个集群数据库，即所有实例及其启用的服务，请输入以下SRVCTL命令：

```纯文本
$ srvctl start database -d db_unique_name [-o start_options]
$ srvctl stop database -d db_unique_name [-o stop_options]
```

例如，以下SRVCTL命令挂载甲骨文RAC数据库的所有未运行实例：

```纯文本
$ srvctl 启动数据库 -d orcl -o 挂载
```

# dministering Oracle ASM Instances with SRVCTL in Oracle RAC

You can use the Server Control Utility (SRVCTL) to add or remove an Oracle ASM instance. To issue SRVCTL commands to manage Oracle ASM, log in as the operating system user that owns the Oracle Grid Infrastructure home and issue the SRVCTL commands from the bin directory of the Oracle Grid Infrastructure home.

Use the following syntax to add an Oracle ASM instance:

```纯文本
srvctl add asm
```

Use the following syntax to remove an Oracle ASM instance:

```纯文本
srvctl remove asm [-f]
```

You can also use SRVCTL to start, stop, and obtain the status of an Oracle ASM instance as in the following examples.

Use the following syntax to start an Oracle ASM instance:

```纯文本
srvctl start asm [-n node_name] [-o start_options]
```

Use the following syntax to stop an Oracle ASM instance:

```纯文本
srvctl stop asm [-n node_name] [-o stop_options]
```

Use the following syntax to show the configuration of an Oracle ASM instance:

```纯文本
srvctl config asm -n node_name 
```

Use the following syntax to display the state of an Oracle ASM instance:

```纯文本
srvctl status asm [-n node_name]
```
