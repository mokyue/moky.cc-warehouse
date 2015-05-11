title: "解决UnicodeEncodeError: 'ascii' codec can't encode characters in position问题"
date: 2015-03-14 11:20:41
categories:
- Python
tags:
- UnicodeEncodeError
- ascii
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.sina.com.cn/s/blog_64a3795a01018vyp.html](http://blog.sina.com.cn/s/blog_64a3795a01018vyp.html "http://blog.sina.com.cn/s/blog_64a3795a01018vyp.html")

今天把一个列表转换成字符串输出的时候出现了`UnicodeEncodeError: 'ascii' codec can't encode characters in position 32-34: ordinal not in range(128)`问题，使用的是`ulipad`编译器。

**解决方法1：**
在开头加上
```python
import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )
```

**解决方法2：**
使用cmd运行python程序，能正常显示结果
