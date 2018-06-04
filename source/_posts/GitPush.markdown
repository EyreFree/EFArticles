---
layout: post
title: Git 踩坑：Git Push 远端无分支不提示
date: 2017-12-25 10:00:00 -05:00
categories: Git
tag: Git
---

上周遇到一个 Git 配置导致的问题，踩坑过程如下。

## 一. 问题描述

1. 首先找一个远端 Git 仓库，clone 到本地；
2. 在本地新建一个分支 test（名字随意，只要远端不存在这个分支即可）并切换到该分支；
3. 执行 `git push` 命令后会发现终端显示了 `Everything up-to-date`，会让人误以为该分支成功推到了远端；
4. 实际上问题已经出现了，这里 `git push` 指令并没有正确提示我们远端不存在该分支。我们可以检查一下远端 Git 仓库，的确没有把 test 分支推上去；

![](https://user-gold-cdn.xitu.io/2017/12/25/1608c4ec3e087cf1?w=585&h=366&f=png&s=55199)

5. 这个问题有多坑呢？假设没察觉这里回显不对，而是把本地分支删了干别的去了，估计就哭了。

## 二. 问题解决

1. 查了 N 多资料；
2. 对比了 N 多类似案例；
3. 耗费了无数脑细胞；
4. 终于在 [TimothyQiu](http://timothyqiu.com/) 大大告诉我解决方法之后解决了该问题，😂；
5. 问题原因大概是因为 `gitconfig` 中的 参数设置异常导致的，我们可以执行 `git config -l`  命令查看当前的 Git 配置，可以看到 `push.default` 的值为 `matching`：

![](https://user-gold-cdn.xitu.io/2017/12/25/1608c4ec3c4f77f6?w=805&h=381&f=png&s=63849)

6. 用 `git config --global push.default simple` 命令把它改成 `simple` 即可：

![](https://user-gold-cdn.xitu.io/2017/12/25/1608c4ec3c565841?w=804&h=396&f=png&s=69369)

7. 然后执行 `git push` 命令就可以正常获取错误提示信息啦：

![](https://user-gold-cdn.xitu.io/2017/12/25/1608c4ec3c6b61bd?w=518&h=103&f=png&s=11300)

---

本文链接：[http://www.eyrefree.org/2017/12/25/2017-12-25-Git-Push/](http://www.eyrefree.org/2017/12/25/2017-12-25-Git-Push/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
