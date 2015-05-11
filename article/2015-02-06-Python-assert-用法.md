title: Python assert 用法
date: 2015-02-06 13:15:09
categories:
- Python
tags:
- Python
- assert
---
1. `assert`语句用来声明某个条件是真的。
2. 如果你非常确信某个你使用的列表中至少有一个元素，而你想要检验这一点，并且在它非真的时候引发一个错误，那么`assert`语句是应用在这种情形下的理想语句。
3. 当`assert`语句失败的时候，会引发一`AssertionError`。

**测试程序：**
```python
mylist = ['item']
assert len(mylist) >= 1
mylist.pop()
'item'
assert len(mylist) >= 1
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
AssertionError
```
