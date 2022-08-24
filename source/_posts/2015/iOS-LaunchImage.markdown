---
layout: post
title: iOS 设置 Launch Image 启动图片 
date: 2015-06-01 09:00:00 -05:00
categories: Code
tag: iOS
---

# 1、添加图片资源
打开工程，进入 `Images.xcassets`，出现图片资源列表，对列表空白处右击单击，在弹出菜单中选择 `New Launch Image`，出现 `LaunchImage` 的空文件夹，按要求添加若干尺寸的 Launch 图片：

![iOS-LaunchImage-1](/images/2015/iOS-LaunchImage/1.jpg)

# 2、修改工程设置
选中工程名，然后在 Targets 中再次选中，接着选择 General，找到 `App Icons and Launch Images`，将第二项 `Launch Image Source` 设为第一步中创建的 LaunchImage，将第三项 `Launch Screen File` 设为空即可。

![iOS-LaunchImage-2](/images/2015/iOS-LaunchImage/2.jpg)

接下来 Run 一下工程，启动 App 时就可以出现我们上面设置的启动图片啦，😊

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://eyrefree.org/2015/iOS-LaunchImage   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
