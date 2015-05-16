title: QTimer和QThread的调度时间精度
date: 2015-05-15 12:07:45
categories:
- Qt
tags:
- QTimer
- QThread
- 时间精度
- 计时器精度
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/dijunfeng/article/details/7272475](http://blog.csdn.net/dijunfeng/article/details/7272475 "http://blog.csdn.net/dijunfeng/article/details/7272475")

<br>
最近做的一个模拟嵌入式设备的项目中，要求事件的响应精度在1毫秒左右，特地编写代码测试了一下QTimer和QThread中的msleep函数的时间精度。

QT的帮助中对于QTimer的时间精度问题是这么写的：
> Timers will never time out earlier than the specified timeout value and they are not guaranteed to time out at the exact value specified. In many situations, they may time out late by a period of time that depends on the accuracy of the system timers.
> 
> The accuracy of timers depends on the underlying operating system and hardware. Most platforms support a resolution of 1 millisecond, though the accuracy of the timer will not equal this resolution in many real-world situations.
> 
> If Qt is unable to deliver the requested number of timer clicks, it will silently discard some.

<br>
测试函数用到了windows的高精度时间读取函数，如下所示:
``` cpp
#include <Windows.h>
#include <math.h>
#define TIMER_INTVL  1000  //毫秒
#define ARRAY_LEN    1  //数组长度

//传入调用时间间隔，打印出最大和平均时间误差
void testTimer(int intvl_us)
{
    static bool inited = false;
    static LARGE_INTEGER lastT;
    static LARGE_INTEGER freq;
    LARGE_INTEGER now;
    static int usarray[ARRAY_LEN];
    static int index = 0;
    static int maxus = 0, averus = 0, difus;//时间差
    QString info("最大时间差：");
    if(!inited)
    {
        memset(usarray, 0, sizeof(int)*ARRAY_LEN);
        QueryPerformanceCounter(&lastT);//获取第一次进入时的时间
        QueryPerformanceFrequency(&freq);//获取时钟频率
        inited = true;
        return;
    }
    QueryPerformanceCounter(&now);
    difus = ((now.QuadPart-lastT.QuadPart)*1000000)/freq.QuadPart;
    difus = abs(difus-intvl_us);
    usarray[index++] = difus;
    maxus = maxus>difus?maxus:difus;
    if(index == ARRAY_LEN)
    {
        index = 0;
        for(int i=0; i<ARRAY_LEN; i++)
            averus += usarray[i];
        averus /= ARRAY_LEN;
        info = info + QString::number(maxus) + "  平均误差 " + QString::number(averus);
        gSimDrvDlg->putInfo(info);
        maxus = 0;
        averus = 0;
    }
    lastT = now;
}
```
<br>
把此函数设为QTimer的超时响应函数，在32位windows7下测试QTimer的不同定时周期的调度误差如下：

**1ms周期：**
*最大：30、40毫秒
平均：100微秒左右*

**10ms周期：**
*最大：2、3毫秒，跳动比较大，也有20毫秒多过
平均：200多微秒*

**100ms周期：**
*最大：20多毫秒
平均：10毫秒左右*

**1秒周期：**
*误差十几毫秒*

把此函数稍加改动，也可以放到QThread的run()函数中测试一下QThread::msleep的时间精度。
在windows下，由于操作系统的本身设计理念问题，定时器的调度误差是比较大的。
