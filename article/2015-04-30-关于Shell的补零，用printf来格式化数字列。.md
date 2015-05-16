title: 关于Shell的补零，用printf来格式化数字列。
date: 2015-04-30 19:00:17
categories:
- Shell
tags:
- printf
- 格式化
- 补零
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://www.187299.com/archives/1680](http://www.187299.com/archives/1680 "http://www.187299.com/archives/1680")

解答论坛一个朋友关于获取01、02...10,而非1、2....10。
因为需要用flashget下载这样一些列的文件。自己了解这个应用，但是以前也没有处理过。还是有需要的。经过g后，测试得到。
``` bash
[root@kook tmp]# cat for.sh
for ((a=1; a<=10 ; a++))
do
printf "%02d\n" $a
done
[root@kook tmp]# ./for.sh
01
02
03
04
05
06
07
08
09
10
```
<br>
这么写，也可以。
``` bash
[root@kook tmp]# cat for.sh
for ((a=1; a<=10 ; a++))
do
printf "%.2d\n" $a
done
```
<br>
继续测试。
``` bash
[root@kook tmp]# printf "%04d\n" -3
-003
[root@kook tmp]# printf "%.4d\n" -3
-0003
```
