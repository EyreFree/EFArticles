---
layout: post
title: Git 常用命令
date: 2015-04-09 10:00:00 -05:00
categories: Code
tag: Git
---

Git 常用命令备忘

---

## SubModule

### 1. 获取所有 SubModule

```bash
git submodule update --init --recursive
```

### 2. 删除某个 SubModule

例如：submodule_xxx

```bash
git submodule deinit submodule_xxx
git rm submodule_xxx
rm -rf .git/modules/submodule_xxx
```

## Tag

### 1. 添加 Tag 并上传本地 Tag 到服务器

例如：2.333

```bash
git tag -a 2.333 -m "2.333 版本的备注信息."
git push origin --tags
```

### 2. 删除本地 Tag 并同步到远程

例如：2.333

```bash
git tag -d 2.333
git push origin :refs/tags/2.333
```

## Branch

### 1. 查看所有分支

```bash
git branch
```

### 2. 同步本地与远程分支

删除远程不存在的本地分支

```bash
git fetch -p
```

## Commit

### 1. 合并本地的最后两次 Commit

```bash
git reset --soft HEAD^git commit --amend
```

### 2. 修改上一次的 Commit 信息

```bash
git commit --amend
```

### 3. 撤销所有未提交的本地修改

```bash
git add -A
git checkout .
git cehckout -f
```

## Remote

### 1. 删除远程仓库地址

```bash
git remote remove origin
```

### 2. 添加远程仓库地址

```bash
git remote add origin https://git.coding.net/eyrefree/xxx.git
```

### 3. Push 本地分支到指定远程分支

例如：Push 本地当前分支到远程仓库 origin 的 master 分支

```bash
git push -u origin master
```

## Other

### 1. 设置本地用户名、邮箱

例如：设置用户名为 eyrefree，邮箱为 eyrefree@eyrefree.org

```bash
git config --global user.name "eyrefree" git config --global user.email eyrefree@eyrefree.org
```

## PS

最后，转载一张觉得挺棒的图片：

![](/images/2015/Git-Commands/git.png)

---

更多 Git 常用命令可参考：[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
或查阅官方文档：[Pro Git book](https://git-scm.com/book/zh/v2)

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://eyrefree.org/2015/Git-Commands   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
