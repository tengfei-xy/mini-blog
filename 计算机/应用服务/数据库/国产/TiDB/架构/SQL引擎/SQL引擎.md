# SQL引擎

## 目录

-   [引擎过程](#引擎过程)
    -   [描述：](#描述)
    -   [基于成本的优化器](#基于成本的优化器)
    -   [构建Online DDL 算法](#构建Online-DDL-算法)
    -   [TiDB-Server](#TiDB-Server)

# 引擎过程

SQL -> AST -> Logical Plan -> Optimized Logical Plan -> Cost Model

Statistics -> CostModel

CostModel -> Selected Phyical Plan

## 描述：

SQL：兼容MySQL语法和语义解析、权限控制和验证

AST：抽象语法树，从文本到结构化的数据，生成AST文件

Logical Plan：等价改写

Optimized Logical Plan：物理优化，基于统计信息与成本生产执行计划，主要优化步骤

Selected Phyical Plan：分发给执行器，寻址和计算

## 基于成本的优化器

-   Power CBO Optimizer
-   Cost
-   task（handle on TiDB or TIDB）

分布式SQL引擎主要优化策略：由TIKV完成预计算，再提交给Server

## 构建Online DDL 算法

1.  TiDB没有分表概念，所以完成DDL过程非常快
2.  把DDL过程分成Public、Delete-onlu、Write-only等几个状态，每状态在多节点之间同步和一致，最终完成DDL

## TiDB-Server

TiDB-Serer是一个对等、无状态的、可横向口占，支持多点写入的，直接承接用户SQL的入口。

前台功能：连接和账号权限管理、MySQL协议编码和解码、独立的SQL执行、库表元信息以及系统变量管理

后台功能：垃圾回收（GC）、执行DDL、统计信息管理、SQL优化器与执行器。
