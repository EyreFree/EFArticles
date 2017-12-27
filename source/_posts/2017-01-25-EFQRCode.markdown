---
layout: post
title: iOS 花式二维码生成和二维码识别
date: 2017-01-25 10:00:00 -05:00
categories: iOS
tag: QRCode
---

iOS 原生的二维码识别非常之棒，反正比 ZXing 和 ZBar 效果都好些，所以以后打算尽量用原生的二维码识别，然后最近把原生的二维码生成也顺便做了一遍，并且在原有基础上加了一些样式参数，封了一个小库方便以后使用。

项目地址：[https://github.com/EyreFree/EFQRCode](https://github.com/EyreFree/EFQRCode)

---

![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/EFQRCode.jpg)

EFQRCode 是一个用 Swift 编写的用来生成和识别二维码的库，它基于系统二维码生成与识别进行开发。

- 生成：利用输入的水印图/图标等资源生成各种艺术二维码；
- 识别：识别率比 iOS 原生二维码识别率更高。

## 一. 效果预览

![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode1.jpg)|![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode2.jpg)|![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode3.jpg)|![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode4.jpg)  
:---------------------:|:---------------------:|:---------------------:|:---------------------:
![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode5.jpg)|![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode6.jpg)|![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode7.jpg)|![](https://raw.githubusercontent.com/EyreFree/EFQRCode/master/assets/QRCode8.jpg)  

## 二. 示例

执行以下命令：

```bash
git clone git@github.com:EyreFree/EFQRCode.git; cd EFQRCode/Example; pod install; open EFQRCode.xcworkspace
```

## 三. 环境

- XCode 8.0+
- Swift 3.0+

## 四. 安装

EFQRCode 可以通过 [CocoaPods](http://cocoapods.org) 进行获取。只需要在你的 Podfile 中添加如下代码就能实现引入：

```
pod "EFQRCode", '~> 1.2.0'
```

## 五. 快速使用

#### 1. 导入 EFQRCode

在你需要使用的地方添加如下代码引入 EFQRCode 模块：

```swift
import EFQRCode
```

#### 2. 二维码识别

获取图片中所包含的二维码，同一张图片中可能包含多个二维码，所以返回值是一个字符串数组：

```swift
if let testImage = UIImage(named: "test.png") {
    if let tryCodes = EFQRCode.recognize(image: testImage) {
        if tryCodes.count > 0 {
            print("There are \(tryCodes.count) codes in testImage.")
            for (index, code) in tryCodes.enumerated() {
                print("The content of \(index) QR Code is: \(code).")
            }
        } else {
            print("There is no QR Codes in testImage.")
        }
    } else {
        print("Recognize failed, check your input image!")
    }
}
```

#### 3. 二维码生成

根据所输入参数创建各种艺术二维码图片，快速使用方式如下:

```swift
// 常用参数:
//                         content: 二维码内容
// inputCorrectionLevel (Optional): 容错率
//                                  L 7%
//                                  M 15%
//                                  Q 25%
//                                  H 30%(默认值)
//                 size (Optional): 边长
//        magnification (Optional): 放大倍数
//                                  (如果 magnification 不为空，将会忽略 size 参数)
//      backgroundColor (Optional): 背景色
//      foregroundColor (Optional): 前景色
//                 icon (Optional): 中心图标
//             iconSize (Optional): 中心图标边长
//       isIconColorful (Optional): 中心图标是否为彩色
//            watermark (Optional): 水印图
//        watermarkMode (Optional): 水印图模式
//  isWatermarkColorful (Optional): 水印图是否为彩色

// 额外参数
//           foregroundPointOffset: 前景点偏移量
//                allowTransparent: 允许透明
```

```swift
if let tryImage = EFQRCode.generate(
    content: "https://github.com/EyreFree/EFQRCode",
    magnification: 9,
    watermark: UIImage(named: "WWF"),
    watermarkMode: .scaleAspectFill,
    isWatermarkColorful: false
) {
    print("Create QRCode image success!")
} else {
    print("Create QRCode image failed!")
}
```

结果：

![生成示例](http://upload-images.jianshu.io/upload_images/1018190-7579f281bbd2d0fd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 六. 使用指南

详情可参见具体使用文档：[https://github.com/EyreFree/EFQRCode/blob/master/README_CN.md](https://github.com/EyreFree/EFQRCode/blob/master/README_CN.md)

## 七. 备注

1. 请选用对比度较高的前景色和背景色组合；
2. 想要提高生成二维码的清晰度可以选择使用 `magnificatio` 替代 `size`，或适当提高它们的数值；
3. 放大倍数过高／边长过大／二维码内容过多可能会导致生成失败；
4. 建议对生成的二维码进行测试后投入使用，例如微信能够扫描成功并不代表支付宝也能成功扫描，请务必根据您的具体业务需要做有针对性的测试；
5. 若有任何问题，期待得到您的反馈，`Issue` 和 `Pull request` 都是受欢迎的。

备注的备注：好用的话可以给个`星星`，蟹蟹，QAQ...

---
本文链接：[http://www.eyrefree.org/2017/01/25/2017-01-25-EFQRCode/](http://www.eyrefree.org/2017/01/25/2017-01-25-EFQRCode/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
