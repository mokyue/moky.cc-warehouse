title: shell编程中的date用法
date: 2015-05-02 17:28:49
categories:
- Shell
tags:
- date
- Linux
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.sina.com.cn/s/blog_61c006ea0100mgxe.html](http://blog.sina.com.cn/s/blog_61c006ea0100mgxe.html "http://blog.sina.com.cn/s/blog_61c006ea0100mgxe.html")

## date --help ##
|||
|:--|:--|
|%%|输出%符号 a literal %|
|%a|当前域的星期缩写 locale’s abbreviated weekday name (Sun..Sat)|
|%A|当前域的星期全写 locale’s full weekday name, variable length (Sunday..Saturday)|
|%b|当前域的月份缩写 locale’s abbreviated month name (Jan..Dec)|
|%B|当前域的月份全称 locale’s full month name, variable length (January..December)|
|%c|当前域的默认时间格式 locale’s date and time (Sat Nov 04 12:02:33 EST 1989)|
|%C|n百年 century (year divided by 100 and truncated to an integer) [00-99]|
|%d|两位的天 day of month (01..31)|
|%D|短时间格式 date (mm/dd/yy)|
|%e|短格式天 day of month, blank padded ( 1..31)|
|%F|文件时间格式 same as %Y-%m-%d|
|%g|the 2-digit year corresponding to the %V week number|
|%G|the 4-digit year corresponding to the %V week number|
|%h|same as %b|
|%H|24小时制的小时 hour (00..23)|
|%I|12小时制的小时 hour (01..12)|
|%j|一年中的第几天 day of year (001..366)|
|%k|短格式24小时制的小时 hour ( 0..23)|
|%l|短格式12小时制的小时 hour ( 1..12)|
|%m|双位月份 month (01..12)|
|%M|双位分钟 minute (00..59)|
|%n|换行 a newline|
|%N|十亿分之一秒 nanoseconds (000000000..999999999)|
|%p|大写的当前域的上下午指示 locale’s upper case AM or PM indicator (blank in many locales)|
|%P|小写的当前域的上下午指示 locale’s lower case am or pm indicator (blank in many locales)|
|%r|12小时制的时间表示（时:分:秒,双位） time, 12-hour (hh:mm:ss [AP]M)|
|%R|24小时制的时间表示 （时:分,双位）time, 24-hour (hh:mm)|
|%s|自基础时间 1970-01-01 00:00:00 到当前时刻的秒数 seconds since `00:00:00 1970-01-01 UTC’ (a GNU extension)|
|%S|双位秒 second (00..60); the 60 is necessary to accommodate a leap second|
|%t|横向制表位(tab) a horizontal tab|
|%T|24小时制时间表示 time, 24-hour (hh:mm:ss)|
|%u|数字表示的星期（从星期一开始 1-7）day of week (1..7); 1 represents Monday|
|%U|一年中的第几周星期天为开始 week number of year with Sunday as first day of week (00..53)|
|%V|一年中的第几周星期一为开始 week number of year with Monday as first day of week (01..53)|
|%w|一周中的第几天 星期天为开始 0-6 day of week (0..6); 0 represents Sunday|
|%W|一年中的第几周星期一为开始 week number of year with Monday as first day of week (00..53)|
|%x|本地日期格式 locale’s date representation (mm/dd/yy)|
|%X|本地时间格式 locale’s time representation (%H:%M:%S)|
|%y|两位的年 last two digits of year (00..99)|
|%Y|年 year (1970…)|
|%z|RFC-2822 标准时间格式表示的域 RFC-2822 style numeric timezone (-0500) (a nonstandard extension)|
|%Z|时间域 time zone (e.g., EDT), or nothing if no time zone is determinable|

By default, date pads numeric fields with zeroes. GNU date recognizes
the following modifiers between `%’ and a numeric directive.
**'-' (hyphen) do not pad the field
'_' (underscore) pad the field with spaces**

<br>
## 一些用法 ##
### 以yymmdd的格式输出43天前的当前时刻 ###
`date +%Y%m%d --date='43 days ago'`
<br>
### 测试十亿分之一秒 ###
`date +'%Y%m%d %H:%M:%S.%N'`
<br>
### 创建以当前时间为文件名的目录 ###
``mkdir `date +%Y%m%d` ``
<br>
### 备份以时间做为文件名的 ###
``tar -cvf ./htdocs`date +%Y%m%d`.tar ./*``
<br>
### 显示时间后跳行，再显示目前日期 ###
`date +%T%n%Y%m%d`
<br>
### 只显示月份与日数 ###
`date +%B%d`
<br>
### 获取上周日期（day,month,year,hour） ###
`date -d "-1 week" +%Y%m%d`
<br>
### 获取24小时前日期 ###
`date --date="-24 hour" +%Y%m%d`
<br>
### shell脚本里面赋给变量值 ###
``date_now=`date +%s` ``
<br>
### 计算执行一段sql脚本的运行时间 ###
``` bash
TIME_BEGIN=$(date '+%s.%N')
$sqlcli < queries/q1.3.sql 1>> $FILE_RESULT  2>> $FILE_ERROR
TIME_END=$(date '+%s.%N')
TIME_RUN=$(awk 'BEGIN{print '$TIME_END' - '$TIME_BEGIN'}')
```
<br>
### 编写shell脚本计算离自己生日还有多少天？ ###
``` bash
read -p "Input your birthday(YYYYmmdd):" date1
m=`date --date="$date1" +%m`    #得到生日的月
d=`date --date="$date1" +%d`    #得到生日的日
date_now=`date +%s`             #得到当前时间的秒值
y=`date +%Y`                    #得到当前时间的年
birth=`date --date="$y$m$d" +%s`      #得到今年的生日日期的秒值
internal=$(($birth-$date_now))        #计算今日到生日日期的间隔时间
if [ "$internal" -lt "0" ]; then             #判断今天的生日是否已过
birth=`date --date="$(($y+1))$m$d" +%s`      #得到明天的生日日期秒值
internal=$(($birth-$date_now))               #计算今天到下一个生日的间隔时间
fi
echo "There is :$((einternal/60/60/24)) days."       #输出结果，秒换算为天
```
<br>
### 若是不以加号作为开头，则表示要设定时间 ###
时间格式`MMDDhhmm[[CC]YY][.ss]`
**MM** 为月份，
**DD** 为日，
**hh** 为小时，
**mm** 为分钟，
**CC** 为年份前两位数字，
**YY** 为年份后两位数字，
**ss** 为秒数
<br>
### 显示目前的格林威治时间 ###
也叫“世界时”。是英国的标准时间，也是世界各地时间的参考标准。中英两国的标准时差为8个小时，即英国的当地时间比中国的北京时间晚8小时。
`date -u`
*Thu Sep 28 09:32:04 UTC 2006*
<br>
### 修改时间 ###
`date -s`
按字符串方式修改时间
可以只修改日期,不修改时间,输入: `date -s 2007-08-03`
只修改时间,输入:date -s 14:15:00
同时修改日期时间,注意要加双引号,日期与时间之间有一空格,输入:date -s "2007-08-03 14:15:00"

修改完后,记得输入:`clock -w`
把系统时间写入CMOS
