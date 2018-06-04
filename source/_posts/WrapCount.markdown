---
layout: post
title: OS X 下统计项目代码行数
date: 2016-07-19 10:00:00 -05:00
categories: OS X
tag: Nothing
---

这是一条普通的计算代码行数的命令，在终端中切换到源码文件所在目录下执行即可：

```
find . "(" -name "*.m" -or -name "*.mm" -or -name "*.swift" -or -name "*.cpp" -or -name "*.h" -or -name "*.rss" ")" -print | xargs wc -l

```

可以计算代码行数，源码文件类型在命令里哦，可以根据自己需要修改，上面这条是计算 iOS 项目的，效果如下：

![Wrap-Count-1.png](/images/Wrap-Count-1.png)


---
本文链接：[http://www.eyrefree.org/2016/07/19/2016-07-19-Wrap-Count/](http://www.eyrefree.org/2016/07/19/2016-07-19-Wrap-Count/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)