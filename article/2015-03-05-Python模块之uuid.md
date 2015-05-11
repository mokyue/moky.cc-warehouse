title: Python模块之uuid
date: 2015-03-05 17:43:46
categories:
- Python
tags:
- uuid
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.csdn.net/zhaoweikid/article/details/1649786](http://blog.csdn.net/zhaoweikid/article/details/1649786 "http://blog.csdn.net/zhaoweikid/article/details/1649786")

`uuid`是一种唯一标识，在许多领域作为标识用途。`python`的`uuid`模块就是用来生成它的。
闲话不说，`python`提供的生成`uuid`的方法一共有4种，分别是：

1. 从硬件地址和时间生成
2. 从md5算法生成
3. 随机生成
4. 从`SHA-1`算法生成

他们在`uuid`模块里对应`uuid1`, `uuid3`, `uuid4`, `uuid5`这几个方法，注意没有`uuid2`。
下面是示例：
```python
#-*- encoding: gb2312 -*-
import uuid

print uuid.uuid1()
print uuid.uuid3(uuid.NAMESPACE_DNS, 'testme')
print uuid.uuid4()
print uuid.uuid5(uuid.NAMESPACE_DNS, 'testme')
```
