---
layout: post
title: iOS 集成广点通移动 App 激活数据统计 API 上报方案
date: 2016-02-18 10:00:00 -05:00
categories: iOS
tag: Nothing
---

iOS 集成广点通移动 App 激活数据统计 API 上报方案。

Demo 地址：[https://github.com/EyreFree/EFGuangDianTongDemo](https://github.com/EyreFree/EFGuangDianTongDemo)

---

# 一，获取参数

## 1，Apple ID

Apple ID 是一个数字，每一个 iOS 应用都有一个 Apple ID，打开 [iTunesConnect](http://itunesconnect.apple.com)，点击我们所需要集成广点通的 App 进入详情页面，点击左边的“App 信息”，找到其中的“综合信息”一项，其中包含我们需要的 Apple ID，如下图所示：

<center>
![iOS-GuangDianTong-1](/images/iOS-GuangDianTong-1.png)
</center>

## 2，UID

UID 是一个数字，它是我们在广点通的账户 ID，打开[广点通](http://e.qq.com)进入管理平台，在最顶部的显示的账户信息中的“账户 ID”就是我们需要的 UID，如下图所示：

<center>
![iOS-GuangDianTong-2](/images/iOS-GuangDianTong-2.png)
</center>

## 3，EncryptKey 和 SignKey

每一个 AppID 广点通会分配给我们一个加密密钥 encrypt_key 和一个签名密钥 sign_key，打开[广点通](http://e.qq.com)进入管理平台，点击左边的“工具箱”然后选择“转化跟踪”，然后点击“创建新转化”，依次输入信息创建对应 App 的转化，注意“转化方案”一项选择“API方案二”，提交后会在列表中出现一个我们新创建的转化，点击“查看”，就会得到我们需要的 encrypt_key 和 sign_key，如下图所示：

<center>
![iOS-GuangDianTong-3](/images/iOS-GuangDianTong-3.png)
</center>

# 二，实现 API 上报方案

根据文档实现了 API 上报方案流程，代码参见：[https://github.com/EyreFree/EFGuangDianTongDemo/blob/master/EFGuangDianTongDemo/EFGuangDianTong.swift](https://github.com/EyreFree/EFGuangDianTongDemo/blob/master/EFGuangDianTongDemo/EFGuangDianTong.swift)

# 三，调用方式

## 1，添加第三方库

需要添加 Alamofire 用于网络操作，Demo 中是通过 CocoaPods 的方式引用，所以在将 Demo Clone 下来后要先进行 pod install 操作，具体内容可参考这篇博文：[CocoaPods安装和使用教程](http://code4app.com/article/cocoapods-install-usage)

## 2，添加头文件

由于实现 API 上报方案的过程中需要用到 MD5 加密，所以需要添加相应的 Objective-C 头文件：

```
#import <CommonCrypto/CommonDigest.h>
```

由于我们这里是 Swift 工程，所以添加 OC 头文件需要通过给项目添加一个用于桥接的头文件，具体过程可参考：[IOS --- OC与Swift混编](http://blog.sina.com.cn/s/blog_8d1bc23f0102v5tl.html)

## 3，添加实现代码

将 Demo 中的 EFGuangDianTong.swift 文件添加到需要集成广点通统计的项目中。

## 4，调用 API 上报方法

在 AppDelegate 的 didFinishLaunchingWithOptions 方法中合适的地方添加如下代码：

```swift
EFGuangDianTong.sharedInstance.Schema2(
      appid: 111111111,              //替换为我们的 Apple ID
      uid: 222222,                   //替换为我们的 UID
      signKey: "xxxxxxxxxxxxxxxx",   //替换为我们的 sign_key
      encryptKey: "zzzzzzzzzzzzzzzz" //替换为我们的 encrypt_key
)
```

## 5，查看返回状态

若上报成功，则 XCode 下方的控制台会输出“广点通上报:成功”；
若失败则会根据返回码输出具体失败原因，可以根据输出的错误信息来做相应的检查。

# 四，备注

集成广点通需要使用 IDFA，请在 App 提交审核时注意勾选相应选项，否则容易导致二进制文件被拒绝。

---
本文链接：[http://www.eyrefree.org/2016/02/18/2016-02-18-iOS-GuangDianTong/](http://www.eyrefree.org/2016/02/18/2016-02-18-iOS-GuangDianTong/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)