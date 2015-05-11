title: "解决 Git 里 fatal: the remote end hung up unexpectedly"
date: 2015-03-02 11:04:24
categories:
- Git
tags:
- fatal
---
>原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。

`fatal: the remote end hung up unexpectedly`
发生在push命令中，有可能是push的文件过大导致。

## **解决方法：** ##
### *for windows* ###
在`.git/config`文件中加入
```
[http]
postBuffer = 524288000
```
<br>
### *for linux* ###
```
git config http.postBuffer 524288000
```
