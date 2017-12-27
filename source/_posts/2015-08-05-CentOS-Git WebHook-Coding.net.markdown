---
layout: post
title: CentOS Git WebHook Coding.net
date: 2015-08-05 10:00:00 -05:00
categories: Git
tag: Nothing
---

利用 Coding.net 项目的 webhook 实现代码 push 后的自动部署。

---
大致思路:
本地 Push 到 Coding 后调用 WebHook 地址对应的 PHP 脚本，PHP 脚本将刚 Push 的版本 Pull 下来实现自动更新。

主要步骤:
1.CentOS 服务器 Clone 项目；
2.编写 PHP 实现调用后 Pull, 这里用 SSH 方式会方便一点；
3.PHP 关闭安全模式/开启 sudo /打开某些函数执行权限…巴拉巴拉反正是开放权限使之能够正常 Pull；
4.Coding 填写 WebHook 地址为上面写的 PHP，模式设为 Push；
5.测试一下，大概好了。

---
参考资料:
[apache/Nginx下的PHP/Ruby执行sudo权限的系统命令](http://www.4wei.cn/archives/1001469)
[shell_exec and git pull](http://stackoverflow.com/questions/5144039/shell-exec-and-git-pull)
[通过sudo解决php执行linux脚本的权限问题](http://blog.csdn.net/wuhengwudi/article/details/7454094)
[PHP 执行 system、exec 等函数发生错误](http://blog.csdn.net/agoago_2009/article/details/8266942)
[php 执行shell命令的函数](http://my.oschina.net/u/190107/blog/86519)
[Git SSH Key 生成步骤](http://blog.csdn.net/hustpzb/article/details/8230454/)

---
本文链接：[http://www.eyrefree.org/2015/08/05/2015-08-05-CentOS-Git%20WebHook-Coding.net/](http://www.eyrefree.org/2015/08/05/2015-08-05-CentOS-Git%20WebHook-Coding.net/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
