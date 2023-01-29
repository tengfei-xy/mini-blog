date +%Y%m%d     //显示现在天年月日
date +%Y%m%d --date="+1 day" //显示后一天的日期
date +%Y%m%d --date="-1 day" //显示前一天的日期
date +%Y%m%d --date="-1 month" //显示上一月的日期
date +%Y%m%d --date="+1 month" //显示下一月的日期
date +%Y%m%d --date="-1 year" //显示前一年的日期
date +%Y%m%d --date="+1 year" //显示下一年的日期

输出前d天的日期

```bash
date -d -100day '+%Y%m%d'
```

查看122天后是几月几号的星期几（1是星期一）

```bash
date -d "+122day" "+%F %u" 
```



计算俩个时间的天数差

```bash
THIS_PATH=$(cd `dirname $0`;pwd)
cd $THIS_PATH
##要求传入的数据格式为yyyyMMdd的两个开始和结束参数，如20160901 20160910
start=$1
end=$2
##将输入的日期转为的时间戳格式
startDate=`date -d "${start}" +%s`
endDate=`date -d "${end}" +%s`
##计算两个时间戳的差值除于每天86400s即为天数差
stampDiff=`expr $endDate - $startDate`
dayDiff=`expr $stampDiff / 86400`
##根据天数差循环输出日期
for((i=0;i<$dayDiff;i++))
do
    process_date=`date -d "${start} $i day" +'%Y%m%d'`
    ./dosth.sh $process_date
    echo $process_date
done
## 这个一般用于要对调度处理某段较长的日期每天处理同样的业务然后通过该脚本即可循环的进行每天处理，只需在for中增加相应的操作即可,比如调度某个脚本dosth.sh从20160801到20160901每天重新处理
```

