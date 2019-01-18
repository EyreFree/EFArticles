---
layout: post
title: Git 常用命令
date: 2015-04-09 10:00:00 -05:00
categories: Git
tag: Nothing
---

Git 常用命令备忘

---

# 1.获取所有 SubModule

```bash
git submodule update --init --recursive
```

# 2.删除某个 SubModule

例如：xxx

```bash
git submodule deinit xxx
git rm xxx
```

# 3.添加 Tag

例如：2.333

```bash
git tag -a 2.333 -m "2.333 版本的备注信息."
```

# 4.上传本地 Tag 到服务器

```bash
git push origin --tags
```

# 5.删除本地 Tag

例如：2.333

```bash
git tag -d 2.333
```

这时可以趁机同时删除远程 Tag

```bash
git push origin :refs/tags/2.333
```

# 6.同步本地与远程分支

删除远程不存在的本地分支

```bash
git fetch --p
```

# 7.合并本地的最后两次 Commit

```bash
git reset --soft HEAD^git commit --amend
```

# 8.修改上一次的 Commit 信息

```bash
git commit --amend
```

# 9.撤销所有未提交的本地修改

```bash
git checkout .
```

# 10.删除远程仓库地址

```bash
git remote remove origin
```

# 11.添加远程仓库地址 

```bash
git remote add origin https://git.coding.net/eyrefree/xxx.git
```

# 12.Push 本地分支到指定远程分支

例如：Push 本地当前分支到远程仓库 origin 的 master 分支

```bash
git push -u origin master
```

# 13.设置本地用户名、邮箱

例如：设置用户名为 eyrefree，邮箱为 eyrefree@eyrefree.org

```bash
git config --global user.name "eyrefree" git config --global user.email eyrefree@eyrefree.org
```

# PS

最后，转载一张觉得挺棒的图片：

![](/images/git.png)

---

更多 Git 常用命令可参考：[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
或查阅官方文档：[Pro Git book](https://git-scm.com/book/zh/v2)

---

本文链接：[http://www.eyrefree.org/2015/04/09/2015-04-09-Git-Commands/](http://www.eyrefree.org/2015/04/09/2015-04-09-Git-Commands/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
