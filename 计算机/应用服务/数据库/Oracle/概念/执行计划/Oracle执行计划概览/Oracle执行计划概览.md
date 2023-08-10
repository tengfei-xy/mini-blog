# Oracle执行计划概览

## 目录

-   [准备工作（用户授权与开启）](#准备工作用户授权与开启)
    -   [方式1：使用SQL审核工具查看执行计划](#方式1使用SQL审核工具查看执行计划)
    -   [方式2：使用命令（以AUTOTRACE方式）](#方式2使用命令以AUTOTRACE方式)
-   [计划报告解说](#计划报告解说)
    -   [查询计划的列说明](#查询计划的列说明)
    -   [Operation列](#Operation列)
    -   [路径访问的顺序](#路径访问的顺序)

执行计划是SQL优化的基础。在执行（查询、插入等）语句之前，优化器会将分析语句并按照一定的顺序，分步骤完成执行。通过分析执行计划可以知道到底执行了什么内容，进而开始SQL优化第一步。那么问题来了，如何看？在哪看？

## 准备工作（用户授权与开启）

### 方式1：使用SQL审核工具查看执行计划

在“线上审核”中查看“风险SQL”，根据“SQL\_ID”，点击具体风险SQL。

!\[image-20210628095059227]\(/Users/tengfei/Library/Application Support/typora-user-images/image-20210628095059227.png)

在“SQL详情”中点击“执行计划”进行查看。

!\[image-20210628095134592]\(/Users/tengfei/Library/Application Support/typora-user-images/image-20210628095134592.png)

### 方式2：使用命令（以**AUTOTRACE**方式）

使用DBA权限的用户引用plustrce.sql脚本进行创建plustrace角色

```sql
SYS@oracle> @?/sqlplus/admin/plustrce.sql
```

授权给**目标问题表**的用户

```sql
SYS@oracle> grant plustrace to tengfei;
Grant succeeded.
```

连接到**目标问题表**的用户后开启AUTOTRACE

```sql
TENGFEI@oracle>  set autotrace on
```

最后执行SQL语句，如下方面命令所示

```纯文本
TENGFEI@oracle> select a from b where a>9997 and b=9999 order by a desc;
```

其他选项：

能够查看**SQL语句的执行结果**和**执行计划**、**统计信息**：`SET AUTOTRACE ON`

只显示**统计信息**：`SET AUTOTRACE ON STATISTICS`

包含**查询计划**和统计信息，不包含查询输出：`AUTOTTRACE TRACEONLY`

只显示**优化器执行路径报告**：`SET AUTOTRACE TRACEONLY EXPLAIN`

关闭：`SET AUTOTRACE OFF`

## 计划报告解说

```sql
Execution Plan
----------------------------------------------------------
Plan hash value: 1078481729

--------------------------------------------------------------------------------------
| Id  | Operation		                 | Name  | Rows  | Bytes | Cost (%CPU)| Time     |
--------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT	           |	     |	   1 |	  26 |	   3   (0)| 00:00:01 |
|*  1 |  TABLE ACCESS BY INDEX ROWID | B     |	   1 |	  26 |	   3   (0)| 00:00:01 |
|*  2 |   INDEX RANGE SCAN DESCENDING| IND_A |	   3 |	     |	   2   (0)| 00:00:01 |
--------------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter("B"=9999)
   2 - access("A">9997)

Note
-----
   - dynamic sampling used for this statement (level=2)


Statistics
----------------------------------------------------------
	  4  recursive calls    -- 递归次数
	  0  db block gets      -- 从缓冲区高速缓存中独缺的总块数
	 38  consistent gets    -- 从缓冲区高速缓存中一个块被请求进行读取的次数
	  1  physical reads     -- 从数据文件到缓存区告诉缓存物理读取的书目
	  0  redo size          -- 语句执行过程中产生的重做信息字节数
	520  bytes sent via SQL*Net to client ongoing -- 从服务器发送到客户端的字节数
	523  bytes received via SQL*Net from client   -- 从客户端接收到的字节数
	  2  SQL*Net roundtrips to/from client        -- 从客户机发送和接收到SQL*Net消息的总数
	  0  sorts (memory)     -- 在内存中排序次数
	  0  sorts (disk)       -- 在磁盘中排序次数
	  1  rows processed     -- 更改或选择返回的行数

```

### 查询计划的列说明

在上图中，本次执行此话包含3条Id。Opearion表示操作内容。Name则表示索引名或表名等信息。Rows表示该ID操作所返回的行数。Bytes是返回的字节数。Cost是优化器花费CPU的成本量化参考。ID前带有`*`星号则表示该条是谓语条件，这一条ID进行的具体操作会展现在Predicate Information中。

### Operation列

`INDEX RANGE SCAN DESCENDING`表示进行的是按递减顺序进行索引范围扫描。

`TABLE ACCESS BY INDEX ROWID`则表示通过索引ROWID扫描表

### 路径访问的顺序

自上而下、从左到右，寻找缩进层次最深且没有子节点的第一个节点开始
