title: Python使用正则表达式获得html标签数据
date: 2015-03-15 20:33:00
categories:
- Python
tags:
- 正则表达式
- 获得html标签数据
---
>原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。

记录一下，获取html标签里面的`14℃`，代码如下：
```python
#!/usr/bin/env python
#-*- coding: utf8 -*-
import re

html = '<div class="w-number"><span class="tpte">14℃</span></div>'

if __name__ == '__main__':
    p = re.compile('<[^>]+>')
    print p.sub('', html)
```
