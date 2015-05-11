title: "开发管理中的版本管理Trunk,Branch和Tags"
date: 2015-01-18 20:14:41
categories:
- SVN
tags:
- SVN
- 开发管理
- 版本控制
- Trunk
- Branch
- Tags
---
>【转】原创作品，允许转载。转载时请务必以超链接形式标明文章原始出处、作者信息和本声明，否则将追究法律责任。
>[http://blog.sina.com.cn/s/blog_49a94d1b0100r7id.html](http://blog.sina.com.cn/s/blog_49a94d1b0100r7id.html "http://blog.sina.com.cn/s/blog_49a94d1b0100r7id.html")

`trunk`:主线，开发过程中的工作目录

`branches`:支线，临时分支，定制化需求
branches/OtasApp001
branches/OtasApp002

`tags`:发布目录，不做修改
tags/release-1.0
tags/release-1.1

`场景一`：
产品开发已经基本完成，并且通过很严格的测试，这时候我们就想发布我们的1.0版本,不再提交代码
`svn copy svn://server/trunk svn://server/tags/release-1.0 -m "1.0 released"`

`场景二`：
有一个客户想对产品做定制，我们可以从已发布库中选择一个版本，做为起点来开发
`svn copy svn://server/tags/release-1.0 svn://server/branches/order009 -m "定单009"`

`场景三`：
有一天，突然在trunk下的core中发现一个致命的bug,那么所有的branches一定也一样，这时需要进行分支合并
1. `svn -r 148:149 merge svn://server/trunk branches/order008`
2. `svn -r 148:149 merge svn://server/trunk branches/order009`
其中`148`和`149`是两次修改的版本号
