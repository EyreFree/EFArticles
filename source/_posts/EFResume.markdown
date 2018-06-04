---
layout: post
title: EFResume - 一个普通的 Swift 简历模板
date: 2017-09-14 10:00:00 -05:00
categories: iOS
tag: Resume
---

![](http://upload-images.jianshu.io/upload_images/1018190-879153e48fb39720.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

EFResume 是一个普通的简历模板（可能还称不上），用 Swift 进行开发，受 [zresume](https://github.com/izuolan/zresume) 启发，因为 zresume 是基于容器技术的然后需要服务器支持，然而对此技术 EyreFree 表示一窍不通并且囊中羞涩但是觉得这份简历真的非常好看呢，所以就只能自己动手改成静态模板了，😂。设计稿来源于 [FREE Resume Template](https://www.behance.net/gallery/15677411/FREE-Resume-Template)。欢迎大家提 Issue 和 PR，希望能和大家一起改进这份简历，然后好用的话望大佬们赏个 Star，🙏，有问题可以来撩我。

> [English Introduction](https://github.com/EyreFree/EFResume/blob/master/README.md)

## 地址

https://github.com/EyreFree/EFResume

## 预览

![](http://upload-images.jianshu.io/upload_images/1018190-f54ae220436583f2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 示例

[https://eyrefree.github.io/EFResume/](https://eyrefree.github.io/EFResume/)

## 环境

- XCode 8.0+
- Swift 3.0+

## 安装

0. 唔，首先需要安装 Xcode；
1. 利用 `git clone` 命令下载本仓库；
2. 随后打开 core 目录下的 `EFResume.xcworkspace` 编译即可。

或执行以下命令：

```bash
git clone git@github.com:EyreFree/EFResume.git; cd EFResume/core; open EFResume.xcworkspace
```

## 使用

1. 用 Xcode 打开工程；
2. 打开 main.swift 文件，编辑 input 函数中对应的文本，将信息修改为自己的即可；
3. 编辑完成后直接编译即可；
4. 打开 docs 目录下的 index.html 可在本地进行预览；
5. 将本地变更提交到远端 Git 仓库；
6. 打开 GitHub 的 Pages 服务，选择 /docs 路径作为根路径，即可生成在线简历同时获得 URL 地址。
7. 祝好运，👍

## 作者

EyreFree, eyrefree@eyrefree.org

## 协议

![](http://upload-images.jianshu.io/upload_images/1018190-4922d1f639709d4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

EFResume 基于 GPLv3 协议进行分发和使用，更多信息参见协议文件。

---

本文链接：[http://www.eyrefree.org/2017/09/14/2017-09-14-EFResume/](http://www.eyrefree.org/2017/09/14/2017-09-14-EFResume/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
