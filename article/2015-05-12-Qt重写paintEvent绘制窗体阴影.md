title: Qt重写paintEvent绘制窗体阴影
date: 2015-05-12 21:57:44
categories:
- Qt
- PyQt
tags:
- paintEvent
- 绘制阴影
---
>原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。

Qt窗体做阴影效果的方法有很多种，有直接使用带阴影效果美术资源的，这里就不详说这种方法了，还有一种方法就是重写paintEvent绘制阴影的方法。
<br>
代码如下: (以下为PyQt代码的实现，C++ Qt实现的方法原理也是一样的。)
``` python
    def paintEvent(self, event):
        """
        绘制阴影
        :param event:
        :return:
        """
        path = QtGui.QPainterPath()
        path.setFillRule(QtCore.Qt.WindingFill)
        path.addRect(10, 10, self.width() - 20, self.height() - 20)
        painter = QtGui.QPainter(self)
        painter.setRenderHint(QtGui.QPainter.Antialiasing, True)
        painter.fillPath(path, QtGui.QBrush(QtCore.Qt.white))
        color = QtGui.QColor(0, 0, 0, 50)
        for i in range(0, 10):
            path = QtGui.QPainterPath()
            path.setFillRule(QtCore.Qt.WindingFill)
            path.addRect(10 - i, 10 - i, self.width() - (10 - i) * 2, self.height() - (10 - i) * 2)
            color.setAlpha(150 - math.sqrt(i) * 50)
            painter.setPen(color)
            painter.drawPath(path)
```
做出来的阴影效果类似于下图:
![效果图](http://i.imgur.com/afURmb0.png)

<br>
之前写的一篇自定义Tooltips的文章用的就是这种方法，大家可以参考一下。
> [绘制支持富文本带阴影自适应大小的Tooltips](http://moky.cc/2015/03/17/%E7%BB%98%E5%88%B6%E6%94%AF%E6%8C%81%E5%AF%8C%E6%96%87%E6%9C%AC%E5%B8%A6%E9%98%B4%E5%BD%B1%E8%87%AA%E9%80%82%E5%BA%94%E5%A4%A7%E5%B0%8F%E7%9A%84Tooltips/)
