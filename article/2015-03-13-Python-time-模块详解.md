title: Python time模块详解
date: 2015-03-13 10:40:07
categories:
- Python
tags:
- time
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/kiki113/article/details/4033017](http://blog.csdn.net/kiki113/article/details/4033017 "http://blog.csdn.net/kiki113/article/details/4033017")

## **简介** ##
time模块提供各种操作时间的函数
说明：一般有两种表示时间的方式:
1. 第一种是时间戳的方式(相对于1970.1.1 00:00:00以秒计算的偏移量),时间戳是惟一的
2. 第二种以数组的形式表示即(struct_time),共有九个元素，分别表示，同一个时间戳的struct_time会因为时区不同而不同

year (four digits, e.g. 1998)
month (1-12)
day (1-31)
hours (0-23)
minutes (0-59)
seconds (0-59)
weekday (0-6, Monday is 0)
Julian day (day in the year, 1-366)
DST (Daylight Savings Time) flag (-1, 0 or 1) 是否是夏令时
If the DST flag is 0, the time is given in the regular time zone;
if it is 1, the time is given in the DST time zone;
if it is -1, mktime() should guess based on the date and time.
夏令时介绍：http://baike.baidu.com/view/100246.htm
UTC介绍：http://wenda.tianya.cn/wenda/thread?tid=283921a9da7c5aef&clk=wttpcts
<br>
## **函数介绍** ##
### asctime() ###
`asctime([tuple]) -> string`
将一个struct_time(默认为当时时间)，转换成字符串
> Convert a time tuple to a string, e.g. 'Sat Jun 06 16:26:11 1998'.
> When the time tuple is not present, current time as returned by localtime()
> is used.

<br>
### clock() ###
`clock() -> floating point number`
该函数有两个功能，
在第一次调用的时候，返回的是程序运行的实际时间；
以第二次之后的调用，返回的是自第一次调用后,到这次调用的时间间隔

示例：
```python
import time  
if __name__ == '__main__':  
    time.sleep(1)  
    print "clock1:%s" % time.clock()  
    time.sleep(1)  
    print "clock2:%s" % time.clock()  
    time.sleep(1)  
    print "clock3:%s" % time.clock()  
```
输出：
clock1:3.35238137808e-006
clock2:1.00004944763
clock3:2.00012040636
其中第一个clock输出的是程序运行时间
第二、三个clock输出的都是与第一个clock的时间间隔
<br>
### sleep(...) ###
`sleep(seconds)`
线程推迟指定的时间运行，经过测试，单位为秒，但是在帮助文档中有以下这样一句话，这关是看不懂
>“The argument may be a floating point number for subsecond precision.”

<br>
### ctime(...) ###
`ctime(seconds) -> string`
将一个时间戳(默认为当前时间)转换成一个时间字符串
例如：
time.ctime()
输出为：'Sat Mar 28 22:24:24 2009'
<br>
### gmtime(...) ###
`gmtime([seconds]) -> (tm_year, tm_mon, tm_day, tm_hour, tm_min,tm_sec, tm_wday, tm_yday, tm_isdst)`
将一个时间戳转换成一个UTC时区(0时区)的struct_time，如果seconds参数未输入，则以当前时间为转换标准
<br>
### localtime(...) ###
`localtime([seconds]) -> (tm_year,tm_mon,tm_day,tm_hour,tm_min,tm_sec,tm_wday,tm_yday,tm_isdst)`
将一个时间戳转换成一个当前时区的struct_time，如果seconds参数未输入，则以当前时间为转换标准
<br>   
### mktime(...) ###
`mktime(tuple) -> floating point number`
将一个以struct_time转换为时间戳
<br>
### strftime(...) ###
`strftime(format[, tuple]) -> string`
将指定的struct_time(默认为当前时间)，根据指定的格式化字符串输出
python中时间日期格式化符号：

|||
|:-:|:--|
|%y|两位数的年份表示（00-99）|
|%Y|四位数的年份表示（000-9999）|
|%m|月份（01-12）|
|%d|月内中的一天（0-31）|
|%H|24小时制小时数（0-23）|
|%I|12小时制小时数（01-12）|
|%M|分钟数（00=59）|
|%S|秒（00-59）|
|%a|本地简化星期名称|
|%A|本地完整星期名称|
|%b|本地简化的月份名称|
|%B|本地完整的月份名称|
|%c|本地相应的日期表示和时间表示|
|%j|年内的一天（001-366）|
|%p|本地A.M.或P.M.的等价符|
|%U|一年中的星期数（00-53）星期天为星期的开始|
|%w|星期（0-6），星期天为星期的开始|
|%W|一年中的星期数（00-53）星期一为星期的开始|
|%x|本地相应的日期表示|
|%X|本地相应的时间表示|
|%Z|当前时区的名称|
|%%|%号本身|
<br>
### strptime(...) ###
`strptime(string, format) -> struct_time`
将时间字符串根据指定的格式化符转换成数组形式的时间
例如：
2009-03-20 11:45:39  对应的格式化字符串为：%Y-%m-%d %H:%M:%S
Sat Mar 28 22:24:24 2009 对应的格式化字符串为：%a %b %d %H:%M:%S %Y
<br>
### time(...) ###
`time() -> floating point number`
返回当前时间的时间戳
<br>
## **疑点** ##
**夏令时**
在struct_time中，夏令时好像没有用，例如
a = (2009, 6, 28, 23, 8, 34, 5, 87, 1)
b = (2009, 6, 28, 23, 8, 34, 5, 87, 0)
a和b分别表示的是夏令时和标准时间，它们之间转换为时间戳应该相关3600，但是转换后输出都为646585714.0
<br>
## **小应用** ##
1. python获取当前时间
time.time() *获取当前时间戳*
time.localtime() *当前时间的struct_time形式*
time.ctime() *当前时间的字符串形式*
   
2. python格式化字符串  
time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) *格式化成2009-03-20 11:45:39形式：*
time.strftime("%a %b %d %H:%M:%S %Y", time.localtime()) *格式化成Sat Mar 28 22:24:24 2009形式：*
 
3. 将格式字符串转换为时间戳
a = "Sat Mar 28 22:24:24 2009"
b = time.mktime(time.strptime(a,"%a %b %d %H:%M:%S %Y"))