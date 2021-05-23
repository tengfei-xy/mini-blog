# TABLE ACCESS FULL

## 概念

执行权标扫描时，服务器进程连续读取高水平线线所有块。

当一个表不断数据插入时high water mark（HWM）值不断提高，全表扫描selete是以hight water mark为终点。

## 

而当表中含有大量过空或者接近空的快时就会导致次优的性能。场景：删除多余插入时。需要执行行迁移