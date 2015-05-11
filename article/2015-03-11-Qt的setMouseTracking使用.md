title: Qt的setMouseTracking使用
date: 2015-03-11 09:38:30
categories:
- Qt
tags:
- Qt
- setMouseTracking
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/marlene0312/article/details/5221261](http://blog.csdn.net/marlene0312/article/details/5221261 "http://blog.csdn.net/marlene0312/article/details/5221261")


`bool mouseTracking`: 这个属性保存的是窗口部件跟踪鼠标是否生效。

如果鼠标跟踪失效（默认），当鼠标被移动的时候只有在至少一个鼠标按键被按下时，这个窗口部件才会接收鼠标移动事件。

如果鼠标跟踪生效，如果没有按键被按下，这个窗口部件也会接收鼠标移动事件。

 

也可以参考`mouseMoveEvent()`和`QApplication::setGlobalMouseTracking()`。

通过`setMouseTracking()`设置属性值并且通过`hasMouseTracking()`来获得属性值。

调用这个函数后，如想使`mouseMoveEvent`有效，也就是在鼠标在区域内移动就会触发，而非鼠标按键按下时才触发，注意只能是`QWidget`，如果是`QMainwindow`，则无效。
