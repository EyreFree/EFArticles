---
layout: post
title: 如何实现通过 NFC 标签唤起 App Clips
date: 2020-09-27 10:00:00 -05:00
categories: iOS
tag: Swift
---

Apple 在 WWDC2020 推出 App Clips 之后引起了不少相关的讨论，很多同学对这种 iOS 平台的 `小程序` 感到很好奇，下面我们就来看看如何通过 NFC 标签来唤起 App Clips 吧。

## NFC 简介

NFC 是 Near Field Communication 的首字母缩写，即近场通信，也被称为近距离无线通信，是一种短距离的高频无线通信技术，通过在单一芯片上集成感应式读卡器、感应式卡片和点对点通信的功能，利用移动设备实现移动支付、电子票务、门禁、移动身份识别、防伪等功能。

![一种 NFC 标签](/images/NFC-AppClips-1.jpg)

### 1. 起源

大约在 2003 年，索尼公司和飞利浦半导体（现为恩智浦 NXP 半导体）进行合作，计划基于 RFID 研发一种更加安全快捷并且能与之兼容的无线通讯技术，不久后双方联合对外发布了一种兼容 IS014443 非接触式卡协议的无线通讯技术，取名为 NFC，具体通信规范称作 [NFCIP-1](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-340.pdf) 规范。随后，索尼、飞利浦与诺基亚一起，创建 NFC 论坛，开始推广 NFC 的应用。

### 2. 原理

NFC 是一种短距高频的无线电技术，NFCIP-1 标准规定 NFC 的通信距离为 10 厘米以内，运行频率 13.56 MHz，传输速度有106 Kbit/s、212 Kbit/s 或者 424 Kbit/s 三种。NFCIP-1 标准详细规定 NFC 设备的传输速度、编解码方法、调制方案以及射频接口的帧格式，此标准中还定义了 NFC 的传输协议，其中包括启动协议和数据交换方法等。

NFC 工作模式分为被动模式和主动模式：

- 被动模式

NFC 发起设备（也称为主设备）利用供电设备的能量来提供射频场，并将数据发送到 NFC 目标设备（也称作从设备）。根据电磁感应原理，从设备利用主设备产生的射频场转换为电能，为从设备的电路供电，接收主设备发送的数据，并且以相同的速度将从设备数据传回主设备。

- 主动模式

发起设备和目标设备在向对方发送数据时，都必须主动产生射频场，所以称为主动模式，它们都需要供电设备来提供产生射频场的能量。这种通信模式可以获得非常快速的连接速率。

### 3. 应用

#### 支付应用

NFC 支付主要是指带有 NFC 功能的手机虚拟成银行卡、一卡通等的应用。

![地铁、公交](/images/NFC-AppClips-2.jpg)

#### 安防应用

NFC 安防的应用主要是将手机虚拟成门禁卡、电子门票等。 NFC 虚拟门禁卡就是将现有的门禁卡数据写入手机的 NFC，使用手机就可以实现门禁功能，门禁的配置、监控和修改等都会变得十分方便，而且可以实现远程修改和配置，例如在需要时临时分发凭证卡等。 

![门禁卡](/images/NFC-AppClips-3.jpg)

#### 标签应用

NFC 标签应用就是把一些信息写入一个 NFC 标签内， 用户只需用 NFC 手机或准备好的设备在 NFC 标签上挥一挥就可以立即获得相关的信息。例如商家可以把含有海报、促销信息、广告的 NFC 标签放在店门口，用户可以根据自己的需求用NFC手机获取相关的信息。

![amiibo](/images/NFC-AppClips-4.jpg)

## App Clips 创建

唔，这里以 [EFQRCode](https://github.com/EFPrefix/EFQRCode/tree/appclips) 的 iOS Demo 为例进行演示。

### 1. 开发环境和设备

首先请安装好 Xcode 12，然后请准备一台已经升级到 iOS 14 的[支持 NFC 的 iOS 设备](https://www.bluebite.com/nfc/iphone-nfc-compatibility)用于测试，我这里是用的是 iPhone X。

|   | Has NFC | Card Emulation | NFC Payments | Reads NFC | Writes NFC |
|--:|:-------:|:--------------:|:------------:|:---------:|:----------:|
| iPhone 11, 11 Pro / Max, SE (2nd Gen) | ✓ | ✓ | ✓ | ✓ | ✓ |
| iPhone XS, XS Max, XR | ✓ | ✓ | ✓ | ✓ | ✓ |
| iPhone X, 8, 8+, 7, 7+ | ✓ | ✓ | ✓ | ✓* | ✓ |
| iPhone 6, 6+, 6S, 6S+, SE (1st Gen) | ✓ | ✓ | ✓ | ✗ | ✗ |
| iPhone 5S, 5C, 5, 4S, 4, 3GS, 3G | ✗ | ✗ | ✗ | ✗ | ✗ |

### 2. 创建 App Clip Target

切换到 Project 的 Targets 列表，点击左下角 `+` 号找到 `App Clip` 项目，添加 Target：

![创建 Target](/images/NFC-AppClips-5.png)

### 3. apple-app-site-association

扫描 NFC 标签打开对应 App Clips / App 实际上是读取了 NFC 标签数据区的 URL 然后打开该 URL 绑定的 App Clips / App，听起来是不是和 Universal Link 很像？所以类似地，需要在想关联的域名下添加 `apple-app-site-association` 文件，并在其中写入 `appclips` 对应的内容，`apps` 值格式为 `DevelopmentTeamID.BundleID`（DevelopmentTeamID 和 BundleID 请替换为你自己的），下面是 [EFQRCode 的](http://efqrcode.com/apple-app-site-association)：

```
{
    "appclips": {
        "apps": ["P3X2725LYY.AppStore.EFQRCode.Clip"]
    }
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "P3X2725LYY.AppStore.EFQRCode",
                "paths": [ "*" ]
            }
        ]
    }
}
```

同时我们切换到刚创建的 App Clip 的 `Signing & Capabilities` 选项卡下，点击左上角的 `+ Capability` 添加 `Associated Domains`，EFQRCode 的如下：

![Associated Domains](/images/NFC-AppClips-6.png)

### 4. Hello World

此时我们在 Xcode 左上角选择 Clip 的 Scheme 进行编译，可以启动一个带有 Hello World 字样的空的 App Clip。

### 5. 添加代码获取唤起 Clip 的 URL

打开我们创建的 App Clip，找到入口文件 `appclipApp.swift` 然后添加如下代码，这里使用的是 SwiftUI 方式进行开发：

```swift
import SwiftUI

@main
struct appclipApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .onContinueUserActivity(NSUserActivityTypeBrowsingWeb, perform: { userActivity in
                    guard let incomingURL = userActivity.webpageURL,
                          let components = NSURLComponents(url: incomingURL, resolvingAgainstBaseURL: true),
                          let queryItems = components.queryItems else {
                        return
                    }
                    print(incomingURL)
                    print(components)
                    print(queryItems)
                })
        }
    }
}
```
## App Clip 的 NFC 唤起

### 1. 制备 NFC 标签

在唤起 Clip 之前，我们需要先制备能够唤起 Clip 的 NFC 标签，同学们可以在淘宝搜索 `NFC 标签贴` 买到空白的 NFC 标签，我这边准备的是 `ISO 14443-3A` 类型的 NFC 便利贴，小小的，可以贴在玩具底座上，如下图所示：

![NFC 便利贴](/images/NFC-AppClips-7.jpg)

刚买到的 NFC 标签是空白的，并不包含数据，所以用 iPhone 贴近 NFC 标签并不会有任何响应。接下来我们打开 App Store 搜索 NFC Tools 随便找一个 NFC 读写器来完成我们对 NFC 标签的读写，这边我是用的是 [NFC Tools](https://apps.apple.com/cn/app/nfc-tools/id1252962749)。

打开 NFC Tools 选择 `读` 选项，当 NFC 扫描弹框出现后，将手机的背部摄像头位置靠近我们准备好的 NFC 标签，即可读出标签内的数据（如果没有识别到的话，可以尝试调整位置或扫描姿势，反正我用的 iPhone X 的 NFC 感应器是在摄像头旁边一点的样子），可以看到目前 NFC 标签内是没有写入任何数据的，只有一些基本信息，并且是可写状态，如下图所示：

![读取空白 NFC 标签内的数据](/images/NFC-AppClips-8.png)

回到 NFC Tools 首页，选择 `写` -> `添加记录` -> `URL / URI`，将我们上面配好的 Associated Domains 地址填入，然后回到写数据页面，点击 `写 / 17 字节` 选项，再将手机贴近 NFC 标签，等待写入成功。这时，我们再使用 `读` 功能读取 NFC 标签内的数据，可以看到我们写入的 URL 记录：

![写入数据](/images/NFC-AppClips-9.png)

### 2. 唤起 Clip

此时我们退出 NFC Tools 回到桌面，将 iPhone 贴近制备好的 NFC 标签，可以看到 iPhone 已经能识别到 NFC 标签内我们写入的 URL，但它并没有唤起我们的 App Clip，而是询问是否用 Safari 打开，这好像和我们的设想有些出入。

![询问是否用 Safari 打开](/images/NFC-AppClips-10.jpg)

唔，这是因为我们的 Clip 并没有上架，所以 iOS 现在并不知道这个 URL 和我们 App Clip 的绑定关系。这个时候我们可以打开 `设置` -> `开发者`，可以看到有 `APP CLIP TESTING` 一项，点击 `Local Experiences` -> `Register Local Experiences` 添加我们的测试数据即可，添加完成后再用 iPhone 靠近 NFC 标签即可成功唤起 Clip：

![唤起 Clip](/images/NFC-AppClips-11.jpg)

唤起成功啦，接下来就可以动手给 Clip 添加内容了呢。

## 后记

本文代码可从 https://github.com/EFPrefix/EFQRCode/tree/appclips 获取，有一些上文没有提到但需要注意的点：

- 一个 App 能且只能拥有一个 App Clip；
- App Clip 的包大小上限为 10M，如何合理封装、复用现有代码，需要花一些时间思考；
- 如果用户已经安装了 App，那么会直接唤起 App 而不是 Clip。

那么问题来了，实际生活中有哪些业务需求可以用到这个酷炫的能力呢？小伙伴们需要自己思考啦...

本文编写过程中参考了以下文章，在此对原作者们表示感谢：

- [一些关于 App Clips 的笔记](https://onevcat.com/2020/06/first-look-app-clips/)
- [App Clips简介以及demo演示](http://www.cocoachina.com/articles/899697)
- [iPhone NFC Compatibility](https://www.bluebite.com/nfc/iphone-nfc-compatibility)

---
本文链接：[https://www.eyrefree.org/2020/09/27/NFC-AppClips/](https://www.eyrefree.org/2020/09/27/NFC-AppClips/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
