title: "py2exe error: MSVCP90.dll: No such file or directory"
date: 2015-05-05 17:22:08
categories:
- Python
tags:
- py2exe
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/sunny5211/article/details/6431864](http://blog.csdn.net/sunny5211/article/details/6431864 "http://blog.csdn.net/sunny5211/article/details/6431864")

当用py2exe 2.6编译python程序时出现这样的错误，从google上搜到了解决方法，英文网站就不翻译了，直接贴出解决方法：

``` python
#setup.py
from distutils.core import setup
import py2exe
setup(windows=["frame.py"],options = { "py2exe":{"dll_excludes":["MSVCP90.dll"]}})
```
保存为setup.py

然后运行：`python setup.py py2exe` 就可以编译成功了
