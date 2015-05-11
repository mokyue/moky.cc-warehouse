title: Qt mapToGlobal 以及 mapToParent 理解要点
date: 2015-02-27 20:48:32
categories:
- Qt
tags:
- Qt
- mapToGlobal
- mapToParent
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/wangjieest/article/details/8423700](http://blog.csdn.net/wangjieest/article/details/8423700 "http://blog.csdn.net/wangjieest/article/details/8423700")


Qt 控件坐标转换,一整子没接触又忘了.

总结下: 这两个函数都是转换相对坐标系用的. 用另一个坐标系统的坐标值, 来表达当前坐标系统中某个坐标所指向的某个点,

记住: 一定要先确两个坐标系统...再确定一个点...

永远要注意,这些函数都有对象的成员函数.即使不写出来也会有一个this指针.(很大程度上都是忘记这个坐标系统而导致的)

例如 `pWidget->mapToGlobal(QPoint(x,y));`

即 把你在pWidget里面的坐标(x,y) 所表示的点. 用Global的坐标表示. 

程序的转换过程也是比较简单的: `pWidget->globalPos()+QPoint(x,y);`

当然,这里没有 globalPos 这个函数. 

但是我们可以变通一个.即 `pWidget->mapToGlobal(QPoint(0,0)) == pWidget->globalPos();`

把pWidget的0坐标,切换成全局坐标来表达.

那么,相对于pWidget的任何坐标都能切换到全局了.

`mapToParent`也是同理.

至于`mapFromGlobal`和`mapFromParent`只要注意是那两个坐标系统要进行转换.就应该很明确了.

(Qt父窗口有对每个子控件坐标的记录...Qt 后台有对每个控件Global坐标的记录)
