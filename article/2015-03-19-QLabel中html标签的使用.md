title: QLabel中html标签的使用
date: 2015-03-19 16:02:41
categories:
- Qt
tags:
- Qt
- QLabel
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/imxiangzi/article/details/7749529](http://blog.csdn.net/imxiangzi/article/details/7749529 "http://blog.csdn.net/imxiangzi/article/details/7749529")

QLabel中显示的字符串是支持HTML标签的。如果应用好的话会达到很多方便快捷的效果。

举几个例子如下：
（1） 作为标题：
用一个QLabel对象，使其字体加大、加粗、居中，使用如下三个标签即可。
``` html
new QLabel("<font size='+1'><b><p align='center'>标题</p></b></font>", this, "title")
```

（2） 加下划线：
使用`<u></u>`即可实现。

（3） 给局部加样式
还可以对text的部分内容添加标签，以使个别内容使用不同字体、样式，并且不影响整体字体。如，给字加颜色、大小、字体等。
``` html
<font color='#5500ff' size='+1' face='Sans'>被设置了字体</font>
```

（4） 画横线
使用`<hr>`即可实现。

完整代码如下：
``` cpp
#include <qlayout.h>   
#include <qframe.h>   
#include <qlabel.h>   
#include <qfont.h>   
QVBoxLayout *vBox = new QVBoxLayout(this);  
vBox->addWidget(new QLabel("<font size='+1'><b><p align='center'>标题</p></b></font>", this));//标题   
vBox->addWidget(new QLabel("<hr>", this, "hr"));//在标题下面画一道横线   
/* 
//或是用下面的方法 
QFrame *lbHr = new QFrame( this, "line4" ); 
lbHr->setGeometry( QRect( 1, 20, width()-2, 16 ) ); 
//lbHr->setPaletteBackgroundColor( QColor( 222, 199, 241 ) ); 
lbHr->setFrameShape( QFrame::HLine ); 
lbHr->setFrameShadow( QFrame::Sunken ); 
lbHr->setFrameShape( QFrame::HLine ); 
vBox->addWidget(lbHr);//横线 
*/  
vBox->addStretch(1);  
vBox->addWidget(new QLabel("<u>带下划线的label</u>", this));  
QLabel *label = new QLabel("设置字体：<font color='#5500ff' size='+1' face='Sans'>被设置了字体</font>", this);  
//字体加粗，被设置字体部分同样加粗   
QFont font = label->font();  
font.setBold(true);  
label->setFont(font);  
vBox->addWidget(label);  
QLabel *label2 = new QLabel("设置字体未加粗：<font color='#5500ff' size='+1' face='Sans'>被设置了字体</font>", this);  
vBox->addWidget(label2);
```
