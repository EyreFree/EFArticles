---
layout: post
title: XCode 中 Swift / Objective-C / C / C++ 混合编程
date: 2015-09-06 10:00:00 -05:00
categories: Code
tag: Swift
---

Swift 是苹果于2014年 WWDC 发布的一种新的用于编写 iOS 和 OS X 应用的编程语言，可与 Objective-C / C / C++ 进行混合编程。

---
# 1.Objective-C 调用 C
Objective-C 是 C 的超集，所以 Objective-C 完全兼容 C，可以直接在 Objective-C 代码中写 C 代码无需修改。

# 2.Objective-C 调用 C++
Xcode 需要源文件以 .mm 为扩展名，这样才能启动编译器的 Objective-C++ 扩展，在 .mm 文件内可以编写 C++ 代码也可以编写 Objective-C 代码，支持大部分的 C++ 的特性，几乎完全兼容 GNU C/C++。

# 3.Swift 调用 Objective-C
## 1.添加桥接文件
添加一个新的头文件到工程中作为桥接文件，建议命名为 `{project_name}-Bridging-Header.h`，这里我命名为 SwiftMixedDemo-Bridging-Header.h，如图所示：

![XCode-Swift-Objective-C-C-C++-1](/images/2015/XCode-Swift-Objective-C-C-C++/1.png)

## 2.设置 Objective-C Bridging Header
选中工程名，切换到 Build Settings 选项卡，选中 All，在右上角的搜索栏中输入 bridging 找到 `Objective-C Bridging Header` 一项，并将其设为 `{project_name}/{project_name}-Bridging-Header.h`，这里我设为 SwiftMixedDemo/SwiftMixedDemo-Bridging-Header.h，如图所示：

![XCode-Swift-Objective-C-C-C++-2](/images/2015/XCode-Swift-Objective-C-C-C++/2.png)

## 3.添加 Objective-C 文件
将需要引入的 Objective-C 文件添加到项目，然后将相应的头文件添加到桥接文件 SwiftMixedDemo-Bridging-Header.h 中：

![XCode-Swift-Objective-C-C-C++-3](/images/2015/XCode-Swift-Objective-C-C-C++/3.png)

接下来即可正常调用 Objective-C 文件中的代码。

## 4.Swift 调用 C/C++
并且 Swift 不能直接调用 C/C++，但可以通过调用 Objective-C 代码的方式间接调用 C/C++。

---
PS：`{project_name}` 代指工程名。

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://eyrefree.org/2015/XCode-Swift-Objective-C-C-C++   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
