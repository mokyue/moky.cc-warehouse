title: Linux Shell 获取当前正在执行脚本的绝对路径
date: 2015-02-03 11:38:30
categories:
- Linux
tags:
- Shell
- 获取运行路径
---
不知道为什么，以前经常使用的一些Linux命令或者使用技巧经常忘记。虽说在汇丰软件一年半都有使用Linux命令，照常理这些是记得很清楚的，但是我就是依赖笔记。每次忘记了就拿出个笔记看一下，命令复制粘贴。
哎~现在那一年半积累写下的笔记，早已随我的离开而烟消云散。`涉及前公司的保密协议和商业风险，电子版笔记一律不准拷贝出公司电脑。`所以在以后的日子里，我会把我学到的东西在`One Night In Mok's Studio`。
这里记录回以前经常忘记的一条Linux命令：
转自： [http://sexywp.com/bash-how-to-get-the-basepath-of-current-running-script.htm](http://sexywp.com/bash-how-to-get-the-basepath-of-current-running-script.htm "http://sexywp.com/bash-how-to-get-the-basepath-of-current-running-script.htm")

```bash
#!/bin/bash
basepath=$(cd `dirname $0` && pwd)
echo $basepath
```
常见的一种误区，是使用pwd命令，该命令的作用是“print name of current/working directory”，这才是此命令的真实含义，当前的工作目录，这里没有任何意思说明，这个目录就是脚本存放的目录。所以，这是不对的。

另一个误人子弟的答案，是$0，这个也是不对的，这个$0是Bash环境下的特殊变量，其真实含义是：

>Expands to the name of the shell or shell script. This is set at shell initialization. If bash is invoked with a file of commands, $0 is set to the name of that file. If bash is started with the -c option, then $0 is set to the first argument after the string to be executed, if one is present. Otherwise, it is set to the file name used to invoke bash, as given by argument zero.

这个$0有可能是好几种值，跟调用的方式有关系：
>1. 使用一个文件调用bash，那$0的值，是那个文件的名字（没说是绝对路径噢）
>2. 使用-c选项启动bash的话，真正执行的命令会从一个字符串中读取，字符串后面如果还有别的参数的话，使用从$0开始的特殊变量引用（跟路径无关了）
>3. 除此以外，$0会被设置成调用bash的那个文件的名字（没说是绝对路径）

很靠近了，但是还是不对，最后，我们说一下上面的脚本是什么意思，从里往外看：
>**dirname $0** 取得当前执行的脚本文件的父目录
>**cd \`dirname $0\`** 进入这个目录（切换当前工作目录）
>**pwd** 显示当前工作目录（cd执行后的）

由此，我们获得了当前正在执行的脚本的存放路径。
