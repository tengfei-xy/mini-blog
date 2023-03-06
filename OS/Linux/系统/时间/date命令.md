

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
=##要求传入的数据格式为yyyyMMdd的两个开始和结束参数，如20160901 20160910
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

常用格式

```
date "+%F %T"
2023-02-09 15:33:52
```

时间计算

```shell
# 显示后一天的日期
date +%Y%m%d --date="+1 day"
# 显示前一天的日期
date +%Y%m%d --date="-1 day"
# 显示上一月的日期
date +%Y%m%d --date="-1 month"
# 显示下一月的日期
date +%Y%m%d --date="+1 month"
# 显示前一年的日期
date +%Y%m%d --date="-1 year"
# 显示下一年的日期
date +%Y%m%d --date="+1 year"
```



格式

```
%%   a literal %
%a   locale's abbreviated weekday name (e.g., Sun)
%A   locale's full weekday name (e.g., Sunday)
%b   locale's abbreviated month name (e.g., Jan)
%B   locale's full month name (e.g., January)
%c   locale's date and time (e.g., Thu Mar  3 23:05:25 2005)
%C   century; like %Y, except omit last two digits (e.g., 20)
%d   day of month (e.g., 01)
%D   date; same as %m/%d/%y
%e   day of month, space padded; same as %_d
%F   full date; same as %Y-%m-%d
%g   last two digits of year of ISO week number (see %G)
%G   year of ISO week number (see %V); normally useful only with %V
%h   same as %b
%H   hour (00..23)
%I   hour (01..12)
%j   day of year (001..366)
%k   hour, space padded ( 0..23); same as %_H
%l   hour, space padded ( 1..12); same as %_I
%m   month (01..12)
%M   minute (00..59)
%n   a newline
%N   nanoseconds (000000000..999999999)
%p   locale's equivalent of either AM or PM; blank if not known
%P   like %p, but lower case
%r   locale's 12-hour clock time (e.g., 11:11:04 PM)
%R   24-hour hour and minute; same as %H:%M
%s   seconds since 1970-01-01 00:00:00 UTC
%S   second (00..60)
%t   a tab
%T   time; same as %H:%M:%S
%u   day of week (1..7); 1 is Monday
%U   week number of year, with Sunday as first day of week (00..53)
%V   ISO week number, with Monday as first day of week (01..53)
%w   day of week (0..6); 0 is Sunday
%W   week number of year, with Monday as first day of week (00..53)
%x   locale's date representation (e.g., 12/31/99)
%X   locale's time representation (e.g., 23:13:48)
%y   last two digits of year (00..99)
%Y   year
%z   +hhmm numeric time zone (e.g., -0400)
%:z  +hh:mm numeric time zone (e.g., -04:00)
%::z  +hh:mm:ss numeric time zone (e.g., -04:00:00)
%:::z  numeric time zone with : to necessary precision (e.g., -04, +05:30)
%Z   alphabetic time zone abbreviation (e.g., EDT)
```

