---
layout: post
title: 如何将 CocoaPods 库升级到 Swift 4
date: 2017-12-05 10:00:00 -05:00
categories: Swift
tag: CocoaPods
---

## 零. 前言

Swift 版本升级嘛，大家应该都很熟练了，菜单 -> Edit -> Convert -> To Current Swift Syntax...，然后巴拉巴拉一顿操作。emmmn，抱歉，编译过了也不一定能正常使用。

这次 Swift 3 到 Swift 4 的更新和之前的大版本更新相比，已经平滑了很多，相较之前的动辄几百上千个 error，现在用 Xcode 进行 Convert 之后基本上只需要进行少量人工修正即可，不过仍然有一些点需要注意，本文将会对一些常见的坑或者注意点以及解决方法进行讨论。

本文以 [EFCountingLabel](https://github.com/EyreFree/EFCountingLabel) 的 1.0.3 版本和 Xcode 9.0 为例，主要关于原有的 Swift 3 的 CocoaPods 库到 Swift 4 的升级，仍处于 Swift 2 阶段的同学可暂时忽略本文。

## 一. 升级流程

### 1. 查看当前版本

首先用 Xcode 打开工程，看一下当前工程设置的 Swift 版本，如果过低的话可能无法直接 Convert，选中需要转换的 target 搜索 `swift_ver` 即可，如图所示：

![](http://upload-images.jianshu.io/upload_images/1018190-51f2abcb6ffc474c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里 EFCountingLabel 的 Swift 版本为 3.2，如果是 2.x 的话需要自己想办法先转换成 Swift 3.x...

### 2. Xcode 代码转换

接下来，就是利用 Xcode 实现代码转换了，菜单 -> Edit -> Convert -> To Current Swift Syntax...，然后选中需要转换的 target，点击 `Next` 按钮即可：

![](http://upload-images.jianshu.io/upload_images/1018190-7235c3b6ddb5f4fe.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3. 选择转换模式

然后会出现一个转换模式选项，有 `Minimize Inference（recommended）` 和 `Match Swift 3 Behavior` 两个选择，苹果推荐的是第一个选项：

![](http://upload-images.jianshu.io/upload_images/1018190-d603752b86a67dd9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

苹果官方文档对这两个选项的描述如下，大意是：如果选第一个选项，会仅在必要的时候为方法或属性添加 `@objc` 标志，不过大部分工作需要用户（也就是你）手动完成，好处是能减少最终生成的二进制文件的大小；如果选择第二个选项，则会按 Swift 3 的方式给所有的地方直接添加 `@objc` 标志（关于 `@objc` 标志的介绍大家可以参考 Swift 翻译组的[这篇文章](http://swift.gg/2016/04/20/swift-qa-2016-04-20/)），缺点就是不会对生成的二进制文件大小进行优化（也就是跟 Swift 3 一样）：

![](http://upload-images.jianshu.io/upload_images/1018190-0565c3371c173e78.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里我们分几种情况：

1. 如果你的 Swift 库不打算支持 OC 调用的话，选 `Minimize Inference（recommended）`，检查并且保存自动转换结果即可，然后可以直接跳到下一小节，请忽略下面这一大段；
2. 如果你的 Swift 库打算支持 OC 调用，但是开发时间紧迫暂时没时间仔细设置 `@objc` 标志或者对这一点二进制文件体积的缩减并不是十分在意的话，选 `Match Swift 3 Behavior`，检查并且保存自动转换结果即可，然后可以直接跳到下一小节，请忽略下面这一大段；
3. 如果你的 Swift 库打算支持 OC 调用，并且打算用推荐的方式进行优化的话，选 `Minimize Inference（recommended）`，保存更改，然后按下面的操作去做：

```
1. 编译工程；
2. 修正那些提示你需要添加 @objc 标志的警告（请务必修正，不然即使编译能过运行时也可能会出问题）；
3. 修正 Xcode 提示的不需要添加 @objc 标志的代码，持续构建和测试你的代码，直到没有任何警告出现；
4. 打开工程设置；
5. 选中 target，搜索 `@objc` 找到 `Swift 3 @objc Inference` 选项，设为 `Default`。
```

唔，以上这段大概是原文翻译过来的了，官方文档原文如图所示：

![](http://upload-images.jianshu.io/upload_images/1018190-57b6b30c33b54e1f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要注意的是，因为我们这里针对的并不是完整的 iOS 项目，而是 CocoaPods 库，如果你的 OC Demo 没有调用库中需要暴露的功能（或者干脆没有 OC Demo），辣么编译器可能完全不会给你任何提示而是直接通过编译了，直到你某一天在一个 OC 工程中引入这个库才会发现并不能调用到某些方法或获取某些属性。

所以其实麻烦之处在于，编译器并不会给你任何提示，因为编译器也不知道哪些类 / 属性 / 方法需要暴露，哪些需要被优化掉，需要开发人员自己决定并手动添加对应的 `@objc` 标志，总结起来的话有以下几点：

1. 需要在 OC 中调用一个 Swift 4 的类，需要让这个类继承 NSObject 并且在这个类前加上 @objc 标志；
2. 需要在 OC 中调用一个 Swift 4 类的方法，需要在方法前加上 @objc 标志（这里有一个坑，如果是普通的函数调用还好，至少编译器会报错，如果是用 `#selector` 的方式调用的话，能过编译并且在运行时直接找不到对应方法而闪退，建议升完 Swift 4 检查一下所有的 #selector 调用）；
3. 需要在 OC 中访问一个 Swift 4 类的某个属性，需要在属性前加上 @objc 标志（同上，如果是普通属性访问的话编译器会报错，但是 KVC 的话会在运行时找不到属性而崩溃，记得检查...）；
4. 需要在 OC 中访问一个 Swift 4 类的扩展，只要在扩展前加上 @objc 标志，该扩展的属性和方法就都能被调用了。

### 4. 更新 Xcode 设置

1. 如下图所示，根据 Xcode 提示将工程设置进行更新，点击 Warning 后单击 `Perform Changes` 按钮即可；

![](http://upload-images.jianshu.io/upload_images/1018190-450ad5a3dfe1641f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 检查设置，将所有 target 的 `Swift 3 @objc Inference` 设置（如果有的话）改为 `Default`，之前改过的话就不用改了；
3. 搜索 `swift_ver`，可以看到当前的 `Swift Language Version` 已经是 `Swift 4` 了。

![](http://upload-images.jianshu.io/upload_images/1018190-d10ebceafdcfcb6a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

剩下少量方法名变动之类的更新大家可以根据提示自行修改，到这里基本就完成了升级过程，不过先别急，接下来我们看注意事项。

## 二. 注意事项

以下情况必须要给对应的属性或方法添加 `@objc` 标志（当然，他们所在的类肯定也需要添加 `@objc` 标志），不管是通过 OC 还是 Swift 调用：

1. 使用 `@selector()` 或 `#selector()` 方式调用的函数；
2. 使用 KVC 进行访问的属性；
3. 使用 IBOutlet 或者 IBAction 和 StoryBoard 绑定的函数或属性。

这些有部分在官方文档中也有提及：

![](http://upload-images.jianshu.io/upload_images/1018190-940d3152de88724d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三. 一些问题

1. 同一工程的 Pods 库是否可以既有 Swift 3 的也有 Swift 4 的？

Swift 的版本控制粒度在 framework 层面，也就是说同一个工程中不同的 framework 可以是按不同版本的 Swift 进行编译的，所以并不需要等待项目依赖的所有 Pods 库都支持 Swift 4 后再更新，完全可以将已经升级 Swift 4 的库先用起来。

2. `Swift 3 @objc Inference` 选项是干啥的？

在 Swift 4 之前，编译器对 Objective-C 自动提供了一些 Swift 声明。例如，编译器会为 NSObject 子类的所有方法创建 Objective-C 入口点，该机制称为 @objc 推断（@objc Inference）。

在 Swift 4 中，这种自动的 @objc 推断已被废弃，因为生成所有这些 Objective-C 入口点有代价，会增大最终的二进制文件体积。当 `Swift 3 @objc Inference` 设置为 `On` 时，它会按照 Swift 4 之前的模式运行，不进行优化，也就是隐式为我们编写的所有 Swift 代码提供 OC 入口。

但是，当设置为 `On` 时 Xcode 会报一个警告，建议修复这个警告，并将设置切换到 `Default`。新的 Swift 项目的默认为“Default”。可以理解为该项设置为 `On` 时和上文代码转换时选择 `Match Swift 3 Behavior` 选项效果类似。

![](http://upload-images.jianshu.io/upload_images/1018190-f32398e06f3f1538.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 四. 没了

升级完请务必跑一遍整体测试流程，暗坑无数，以防万一，祝大家线上稳定。

---

本文链接：[http://www.eyrefree.org/2017/12/05/2017-12-05-CocoaPods-Swift4/](http://www.eyrefree.org/2017/12/05/2017-12-05-CocoaPods-Swift4/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
