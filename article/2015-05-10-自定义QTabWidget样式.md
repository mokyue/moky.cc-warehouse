title: 自定义QTabWidget样式
date: 2015-05-10 13:27:38
categories:
- Qt
tags:
- QSS
- QTabWidget
---
>原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。

记录一下如果用`QSS`实现如下效果的`QTabWidget`控件:
![效果图](result.png)

``` css
QTabWidget#tabWidget:pane {
    border-width: 0;
    background: #ffffff;
}

QTabBar:tab {
    border-image: url(:/tab-normal.png);
    width: 90px;
    height: 35px;
    color: #999999;
    font: 12px "Microsoft Yahei";
}

QTabBar:tab:selected {
    border-image: url(:/tab-selected.png);
    color: #333333;
}

QTabBar:tab:hover {
    color: #333333;
}
```

图片资源: `PS: 图片资源右边有一条浅灰色的竖线以区分每个Tab，可以调整QTabWidget宽度使最后一个Tab不显示这条竖线 :)`
![tab-normal.png](tab-normal.png)

![tab-selected.png](tab-selected.png)
