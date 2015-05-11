title: Qt开发者关于QThread的咆哮
date: 2015-01-23 21:19:32
categories:
- Qt
tags:
- Qt
- QThread
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.jobbole.com/84149/](http://blog.jobbole.com/84149/ "http://blog.jobbole.com/84149/")

我们（Qt用户）正广泛地使用IRC来进行交流。我在Freenode网站挂出了#qt标签，用于帮助大家解答问题。我经常看到的一个问题（这让我不厌其烦），是关于理解Qt的线程机制以及如何让他们写的相关代码正确工作。人们贴出他们的代码，或者用代码写的范例，而我则总是以这样的感触告终：
**你们都用错了！**

我觉得有件重要的事情得澄清一下，也许有点唐突了，然而，我不得不指出，下面的这个（假想中的）类是对面向对象原则的错误应用，同样也是对Qt的错误应用。
```cplusplus
class MyThread : public QThread
{
public:
    MyThread()
    {
        moveToThread(this);
    }
 
    void run();
 
signals:
    void progress(int);
    void dataReady(QByteArray);
 
public slots:
    void doWork();
    void timeoutHandler();
};
```

我对这份代码最大的质疑在于 moveToThread(this);  我见过太多人这么使用，并且完全不明白它做了些什么。那么你会问，它究竟做了什么？moveToThread()函数通知Qt准备好事件处理程序，让扩展的信号（signal）和槽（slot）在指定线程的作用域中调用。QThread是线程的接口，所以我们是在告诉这个线程在“它内部”执行代码。我们也应该在线程运行之前做这些事。即使这份代码看起来可以运行，但它很混乱，并不是QThread设计中的用法（QThread中写的所有函数都应该在创建它的线程中调用，而不是QThread开启的线程）。

在我的印象中，moveToThread(this);  是因为人们在某些文章中看到并且使用而流传开来的。一次快速的网络搜索就能找到此类文章，所有这些文章中都有类似如下情形的段落：
1. 继承QThread类
2. 添加用来进行工作的信号和槽
3. 测试代码，发现槽函数并没有在“正确的线程”中执行
4. 谷歌一下，发现了moveToThread(this);  然后写上“看起来的确管用，所以我加上了这行代码”

我认为，这些都源于第一步。QThread是被设计来作为一个操作系统线程的接口和控制点，而不是用来写入你想在线程里执行的代码的地方。我们（面向对象程序员）编写子类，是因为我们想扩充或者特化基类中的功能。我唯一想到的继承QThread类的合理原因，是添加QThread中不包含的功能，比如，也许可以提供一个内存指针来作为线程的堆栈，或者可以添加实时的接口和支持。用于下载文件、查询数据库，或者做任何其他操作的代码都不应该被加入到QThread的子类中；它应该被封装在它自己的对象中。

通常，你可以简单地把类从继承QThread改为继承QObject，并且，也许得修改下类名。QThread类提供了start()信号，你可以将它连接到你需要的地方来进行初始化操作。为了让你的代码实际运行在新线程的作用域中，你需要实例化一个QThread对象，并且使用moveToThread()函数将你的对象分配给它。你同过moveToThread()来告诉Qt将你的代码运行在特定线程的作用域中，让线程接口和代码对象分离。如果需要的话，现在你可以将一个类的多个对象分配到一个线程中，或者将多个类的多个对象分配到一个线程。换句话说，将一个实例与一个线程绑定并不是必须的。

我已经听到了许多关于编写Qt多线程代码时过于复杂的抱怨。原始的QThread类是抽象类，所以必须进行继承。但到了Qt4.4不再如此，因为QThread::run()有了一个默认的实现。在之前，唯一使用QThread的方式就是继承。有了线程关联性的支持，和信号槽连接机制的扩展，我们有了一种更为便利地使用线程的方式。我们喜欢便利，我们想使用它。不幸的是，我太晚地意识到之前迫使人们继承QThread的做法让新的方式更难普及。

我也听到了一些抱怨，是关于没有同步更新范例程序和文档来向人们展示如何用最不令人头疼的方式便利地进行开发的。如今，我能引用的最佳的资源是[我数年前写的一篇博客](http://blog.qt.io/blog/2006/12/04/threading-without-the-headache/ "我数年前写的一篇博客")。

免责声明：你所看到的上面的一切，当然都只是个人观点。我在这些类上面花费了很多精力，因此关于要如何使用和不要如何使用它们，我有着相当清晰的想法。

译者注：
最新的Qt帮助文档同时提供了建立QThread实例和继承QThread的两种多线程实现方式。根据文档描述和范例代码来看，若想在子线程中使用信号槽机制，应使用分别建立QThread和对象实例的方式；若只是单纯想用子线程运行阻塞式函数，则可继承QThread并重写QThread::run()函数。

由于继承QThread后，必须在QThread::run()函数中显示调用QThread::exec()来提供对消息循环机制的支持，而QThread::exec()本身会阻塞调用方线程，因此对于需要在子线程中使用信号槽机制的情况，并不推荐使用继承QThread的形式，否则程序编写会较为复杂。

扩展阅读：[QObject 之 Thread Affinity](http://blog.csdn.net/dbzhang800/article/details/6557272 "QObject 之 Thread Affinity")


注：
1. Thread Affinity：线程相关性
2. “删除QThread对象前，确保线程内所有对象都没销毁”一句有误，应为“被销毁”，Qt文档中相关记录为“You must ensure that all objects created in a thread are deleted before you delete the QThread.”
