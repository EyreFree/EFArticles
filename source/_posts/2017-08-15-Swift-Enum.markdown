---
layout: post
title: Swift 流水账：踩到一个 Enum 坑（并不是
date: 2017-08-15 10:00:00 -05:00
categories: Swift
tag: Enum
---

今天，天气晴朗，阳光明媚，我像往常一样赖床赖到了九点半，然后在最后一遍起床闹钟的催促声中穿起了衣服，飞一般地冲出了出租屋，蹬上小区门口的小黄，一路冲刺，在即将迟到的前 1s 到达了工位，和平的日常呢。

![](http://upload-images.jianshu.io/upload_images/1018190-ee973c8209f8dd57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

熟练地打开 XCode（我为什么这么熟练呢...）启动项目，开始继续完成产品大大昨天下达的任务。这时，一个枚举进入了我的视野范围内，枚举常量数据类型是 NSUInteger，哼哼，用表驱动法结合 rawValue 的方式，就能优雅地实现这个需求了，完美。

然而，跑了一下居然发生了运行时错误炸掉了...没道理啊，这也能炸...

![EyreFree 眉头一皱，发现事情并不简单](http://upload-images.jianshu.io/upload_images/1018190-59ea929cb1b4246e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点开这个枚举类型，仔细观察了起来...然后发现了一个坑...（应该是我年少无知...

下面，我带着大家一起跳进这个坑...哦不，一起复现一下这个问题：

### 1. 新建一个 OC 的 Pod 库

首先，我们需要新建一个 OC 的 Pod 库，然后在其中定义一个枚举类型，指定枚举值从 2 开始（反正不要是默认的 0 就行），大概这个样子就行了：

```objc
#import <Foundation/Foundation.h>

typedef NS_ENUM(NSUInteger, TestEnum) {
    TestEnumA = 2,
    TestEnumB,
    TestEnumC,
    TestEnumD,
    TestEnumE,
};

typedef TestEnum EFTestEnumType;
```

![1](http://upload-images.jianshu.io/upload_images/1018190-2529d4f32de43374.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2. 新建一个 Swift 工程

然后，我们再建一个新的 Swift 工程（没错，我司项目是 Swift 的...），在其中引入第一步建好的 CocoaPods 库。到这里，我们可以随便找个地方编写如下测试代码：

```swift
print("\(EFTestEnumType.A.rawValue)")
```
先不要执行蛤，大家按住 command 键点击 EFTestEnumType 进入类型定义可以看到如下代码：

```swift
public enum TestEnum : UInt {
    case A
    case B
    case C
    case D
    case E
}
public typealias EFTestEnumType = TestEnum
```

注意到了么，这里通过 Pod 库中的原始 OC 代码转化出的中间 Swift 代码的枚举中，并没有指定枚举值的起始值。

![2](http://upload-images.jianshu.io/upload_images/1018190-b2e7873bd027cbed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3. 编译运行并观察

然后编译运行，观察测试代码的输出会发现，EFTestEnumType.A.rawValue 的值的确是 2...所以，我在主工程中查看了某个枚举类型的定义，而没有注意到 Pod 库中枚举的原始定义是指定了枚举值的起始值的（很好奇为啥这里不一样，搞这么多幺蛾子...），然后就炸了，数组下标越界，初始化失败，随便来一个都会炸掉了...

小伙伴们看懂了么...（嘛，如果这是常识的话...请告诉我我好删掉这篇水文...逃...

![](http://upload-images.jianshu.io/upload_images/1018190-94efe02f989268af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

PS: 文中所用代码可以在 [https://github.com/EyreFree/EFEnumPitDemo](https://github.com/EyreFree/EFEnumPitDemo) 找到。

---

更新：

感谢 [@kemchenj](http://www.weibo.com/kemchenj) 大大的提示，这里应该需要将鼠标悬浮到枚举值之上才可以查看到对应的原始值，反正我还是觉得坑...[摊手]

> @kemchenj：看了很久之后终于懂了，其实主要是 Interface 的锅，Interface 里不会显示枚举值的具体原始值，跟 OC 转 Swift 无关，你可以在 Swift 里定义一个相同的枚举，然后进 Xcode 菜单 -> Navigate -> Jump To Generated Interface，这样就可以看到这个 swift 文件的 Interface 了，也不会具体的 rawValue

![](http://upload-images.jianshu.io/upload_images/1018190-afadd3defe195ad7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

本文链接：[http://www.eyrefree.org/2017/08/15/2017-08-15-Swift-Enum/](http://www.eyrefree.org/2017/08/15/2017-08-15-Swift-Enum/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
