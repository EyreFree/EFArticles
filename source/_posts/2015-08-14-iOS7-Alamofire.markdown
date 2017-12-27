---
layout: post
title: 在需要兼容 iOS 7.0 及以下的项目中使用 Alamofire
date: 2015-08-14 10:00:00 -05:00
categories: iOS
tag: Alamofire
---

Alamofire 是 iOS 和 OS X 上最受欢迎的第三方库之一，它在 [Github](https://github.com/Alamofire/Alamofire) 上面获得了 15638 个 stars 和2304 个 forks，是使用最广的开源项目之一。

---
# 1.官方描述
```
Embedded frameworks require a minimum deployment target of iOS 8 or OS X Mavericks.
To use Alamofire with a project targeting iOS 7, you must include all Swift files located inside the Source directory directly in your project. See the ‘Source File’ section for additional instructions.
```
官方文档指出在需要兼容 iOS 7 的项目中一定要包含所有 Alamofire 源文件。

# 2.添加方法
## 1.添加 Alamofire 子模块
首先添加 submodule，将 Alamofire 作为当前项目的一个子模块：
```bash
#添加子模块：
git submodule add https://github.com/Alamofire/Alamofire.git Alamofire
#初始化子模块：
git submodule init
#更新子模块：
git submodule update
```

## 2.添加源文件到工程
将 Alamofire 目录下的 Source 目录中的所有 `.swift` 文件以引用方式添加到项目中去，如图所示：

<center>
![iOS7-Alamofire-1](/images/iOS7-Alamofire-1.png)
</center>

# 3.调用方法
直接调用方法即可，不需要通过 `Alamofire.` 前缀，如图所示：

<center>
![iOS7-Alamofire-2](/images/iOS7-Alamofire-2.png)
</center>

---
本文链接：[http://www.eyrefree.org/2015/08/14/2015-08-14-iOS7-Alamofire/](http://www.eyrefree.org/2015/08/14/2015-08-14-iOS7-Alamofire/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
