title: 在QtWebKit.QWebView中显示透明背景的网页
date: 2015-03-04 15:53:39
categories:
- PyQt
tags:
- Qt
- QtWebKit
- QWebView
- 透明网页
---
>原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。

`Qt`中的`QtWebKit`让我们可以轻而易举地在客户端里面嵌入网页，而网页的灵活性远远大于需要编译动作的客户端，当我们有变动只需要发布修改的网页而无需重新编译客户端，大大提高了我们开发和发布的效率。

不过默认的`QtWebKit.QWebView`是不透明的，当我们需要让透明网页页面和客户端更好地整合起来的时候，就需要做一些特殊的处理：

1. 首先，确保网页页面背景已经设为`transparent`；
2. 然后设置`QtWebKit.QWebView`的样式，代码如下：
```python
def transparent_webview():
    app = QtGui.QApplication(sys.argv)
    web = QtWebKit.QWebView()
    web.setAttribute(QtCore.Qt.WA_TranslucentBackground)
    web.setWindowFlags(QtCore.Qt.FramelessWindowHint)
    web.setStyleSheet('QWebView { background: transparent; }')
    web.load(QtCore.QUrl("http://www.moky.cc/transparent.html"))
    web.show()
    sys.exit(app.exec_())
```