title: PyChecker使用指南
date: 2015-05-08 18:51:45
categories:
- Python
tags:
- PyChecker
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/thoughtwise/article/details/5719455](http://blog.csdn.net/thoughtwise/article/details/5719455 "http://blog.csdn.net/thoughtwise/article/details/5719455")

## 简介 ##
PyChecker是一个对Python源代码进行语法检查的工具。
主页：[http://pychecker.sourceforge.net](http://pychecker.sourceforge.net "http://pychecker.sourceforge.net")

PyChecker is a static analysis tool for finding bugs in Python source code. It finds problems that are typically caught by a compiler for less dynamic languages, like C and C++. It is similar to lint.

目前最新版本是2008-08-18 更新的pychecker-0.8.18。

<br>
## 安装 ##
前提：本地安装好Python2.x， 推荐2.6版本，虽然2.7版本也发布了。3.0版差异太大，相当于全新版本，不保证兼容性，不推荐使用。
最好将IPython也一并安装了。

下载地址：[http://sourceforge.net/projects/pychecker](http://sourceforge.net/projects/pychecker "http://sourceforge.net/projects/pychecker")

从网页上下载pychecker-0.8.18.tar.gz，解压。

然后在pychecker-0.8.18目录(其下有setup.py脚本)， 执行命令行`python setup.py install`执行安装。这也是在Windows下python软件的通用安装方式。

<br>
## 使用 ##
安装好后，试试在pychecker-0.8.18目录执行命令行`pychecker setup.py`，检查setup.py的语法
```
E:/pychecker-0.8.18>pychecker setup.py

E:/pychecker-0.8.18>C:/Python26/python.exe C:/Python26/Lib/site-packages/pychecker/checker.py se
tup.py
Processing module setup (setup.py)...

Warnings...

C:/Python26/lib/distutils/command/bdist_wininst.py:271: Statement appears to have no effect

C:/Python26/lib/distutils/command/build_scripts.py:80: No class attribute (dry_run) found
C:/Python26/lib/distutils/command/build_scripts.py:97: No class attribute (dry_run) found
C:/Python26/lib/distutils/command/build_scripts.py:120: (file) shadows builtin
C:/Python26/lib/distutils/command/build_scripts.py:121: No class attribute (dry_run) found

C:/Python26/lib/distutils/command/install_data.py:62: (dir) shadows builtin
C:/Python26/lib/distutils/command/install_data.py:64: (dir) shadows builtin
C:/Python26/lib/distutils/command/install_data.py:66: (dir) shadows builtin

C:/Python26/lib/distutils/command/install_scripts.py:52: (file) shadows builtin
C:/Python26/lib/distutils/command/install_scripts.py:53: No class attribute (dry_run) found

19 errors suppressed, use -#/--limit to increase the number of errors displayed
```
 
这里pychecker 是个bat脚本，实际执行的是C:/Python26/python.exe C:/Python26/Lib/site-packages/pychecker/checker.py 。

这里检查结果将setup.py依赖的文件中语法错误或告警也检查出来了。

如果只想检查setup.py自身的语法，可以用`--only`参数

```
E:/pychecker-0.8.18>pychecker --only setup.py

E:/pychecker-0.8.18>C:/Python26/python.exe C:/Python26/Lib/site-packages/pychecker/checker.py --only setup.py
Processing module setup (setup.py)...

Warnings...

None
```
更多的参数，可以使用`pychecker --help`查看
