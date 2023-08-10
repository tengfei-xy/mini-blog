# TABLE

## 目录

-   [TABLE ACCESS FULL](#TABLE-ACCESS-FULL)
    -   [概念](#概念)

# TABLE ACCESS FULL

## 概念

在执行计划中Operation列的“FULL TABLE SCAN”便是全表扫描。执行全表扫描时，服务器进程连续读取高水平线（high water mark）以下所有块。当一个表不断数据插入时高水平线不断提高，全表扫描中selete是以高水平线为终点。而当表中含有大量过空或者接近空的快时就会导致次优的性能。场景：删除多余插入时。需要执行行迁移。
