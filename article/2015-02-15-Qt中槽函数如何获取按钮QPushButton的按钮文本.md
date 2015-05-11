title: Qt中槽函数如何获取按钮QPushButton的按钮文本
date: 2015-02-15 20:29:01
categories:
- Qt
tags:
- Qt
- 信号
- 槽
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/sowhat_ah/article/details/43483557](http://blog.csdn.net/sowhat_ah/article/details/43483557 "http://blog.csdn.net/sowhat_ah/article/details/43483557")

Qt中的信号-槽机制大大降低了编程的耦合度；

QPushButton是按钮中最常用的一个组件；

但是QPushButton的几个信号中除了缺省参之外都没有带参：
```cplusplus
Q_SIGNALS:  
    void pressed();  
    void released();  
    void clicked(bool checked = false);  
```
<br>
也就是说，若你的槽函数与多个QPushButton的clicked()信号相连，则你的槽函数根本无法区分是哪个QPushButton发出的信号；

这在很多时候用起来也是很不方便；

“软件开发中遇到的所有问题，都可以通过增加一层抽象而得以解决”

同样的，这个问题我们可以通过子类化来解决：
```cplusplus
class KeyButton : public QPushButton  
{  
    Q_OBJECT  
public:  
    explicit KeyButton(QWidget *parent = 0) : QPushButton(parent),  
        pauseMsecs(400), intervalMsecs(30)  
    {  
        tm = new QTimer(this);  
        connect(tm, SIGNAL(timeout()), this, SLOT(on_pressed_last()));  
        connect(this, SIGNAL(pressed()), this, SLOT(on_pressed()));  
        connect(this, SIGNAL(released()), this, SLOT(on_released()));  
        connect(this, SIGNAL(clicked()), this, SLOT(on_clicked()));  
    }  
  
private:  
    QTimer *tm;  
    long pauseMsecs;  
    long intervalMsecs;  
  
signals:  
    void keyPressed(const QString &msg);  
    void keyReleased(const QString &msg);  
    void keyClicked(const QString &msg);  
  
public slots:  
    void on_pressed() { emit this->keyPressed(this->text());  
                        tm->start(pauseMsecs); }  
    void on_pressed_last() { emit this->keyPressed(this->text());  
                             tm->setInterval(intervalMsecs); }  
    void on_released() { tm->stop(); emit this->keyReleased(this->text()); }  
    void on_clicked() { emit this->keyClicked(this->text()); }  
}; 
```
<br>
这其中的定时器，是为了实现类似长按则连续输入的按键效果；

这样的按钮类就可以在自己的信号Clickied()、Pressed()或Released()中夹带各种信息，使得槽函数能根据信息不同进行不同处理，这里夹带的是按钮上的文本，你也可以根据需求随意修改，夹带各种你想传递的信息。
