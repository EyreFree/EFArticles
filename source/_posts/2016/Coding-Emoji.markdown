---
layout: post
title: 十分钟开发一款 iOS 表情包 App
date: 2016-11-24 10:00:00 -05:00
categories: Code
tag: Sticker
---

在最近更新的 iOS 10 系统中，苹果开放了 iMessage Stickers 的开发，通俗的说法就是我们现在可以为 iMessage 开发表情包了。
表情包的开发十分简单，不需要写一行代码，只需要准备好图片资源即可。本文主要以 Coding 的[洋葱猴系列表情](https://coding.net/u/coding/p/Onion-Monkey-Emoji/git)为例快速开发一款表情包 App。

本文所需所有素材以及工程文件地址：  
[https://github.com/EyreFree/CodingEmoji](https://github.com/EyreFree/CodingEmoji)

---
# 一.注意事项
开发环境：XCode 8.0 及以上；
运行环境：iOS 10 及以上；
其他：表情包图片的格式可以是 JPG, PNG, GIF 等，不过单张图片最大不能超过 500KB。

# 二.准备图片
下载洋葱猴表情包，找到其中的表情图片。
（下载地址：https://coding.net/u/coding/p/Onion-Monkey-Emoji/git）

![洋葱猴表情包](/images/2016/Coding-Emoji/1.png)

# 三.建立工程
## 1.新建工程
打开 XCode，新建 Sticker Pack Application 工程，如图所示：

![新建 Sticker Pack Application 工程](/images/2016/Coding-Emoji/2.png)

## 2.添加图标
Sticker Pack Application 的图标和一般的 iOS 应用不太一样，它部分图标是扁的，详细尺寸如下（最后一个为 App Store 需要上传的图标尺寸。其他为工程内用到的应用图标）：

| No | Size | PS |
|:---:|:-------------:|:-----:|
| 1 | 54 x 40 ||
| 2 | 58 x 58 ||
| 3 | 64 x 48 ||
| 4 | 81 x 60 ||
| 5 | 87 x 87 ||
| 6 | 96 x 72 ||
| 7 | 120 x 90 ||
| 8 | 134 x 100 ||
| 9 | 148 x 110 ||
| 10 | 180 x 135 ||
| 11 | 1024 x 768 ||
| 12 | 1024 x 1024 | App Store 应用图标 |

![应用图标](/images/2016/Coding-Emoji/3.png)

## 3.导入表情图片
接下来，可以将我们想要添加到表情包里的图片拖到 Sticker Pack 目录中，如图所示：

![导入表情图片](/images/2016/Coding-Emoji/4.png)

## 4.修改表情包名称
我们可以通过修改 Display Name 的方式来修改表情包在设备上显示的名称：

![修改表情包名称](/images/2016/Coding-Emoji/5.png)

# 四.测试
完成上面这些步骤后，就可以编译然后进行测试了，模拟器中运行效果如图所示：

![最终运行效果](/images/2016/Coding-Emoji/6.png)

# 五.提交审核
（这一步不算在 10 分钟里哦，不属于开发过程唉，顺带提一下凑字数，🙄，购买开发者账号要等好几天呢）
若已经准备好了 iOS 开发者账号，就可以直接提交审核了，嗯，这个时候需要准备两张运行效果的屏幕截图，分别是 iPhone 和 iPad 的。

| Device | Size |
|:---:|:-------------:|
| iPhone | 1242 x 2208 |
| iPad | 2048 x 2732 |

然后应用图标使用之前准备好的 1024 x 1024 的应用图标即可，接下来填写好应用的各种信息，然后存储-提交审核即可。

本文 App 已经通过审核，iOS 10 以上系统的同学可以下载体验：
[https://itunes.apple.com/cn/app/yang-cong-hou-biao-qing-bao/id1166254758?mt=8](https://itunes.apple.com/cn/app/yang-cong-hou-biao-qing-bao/id1166254758?mt=8)

同时预祝各位同学顺利开发出属于自己的表情包应用。

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://eyrefree.org/2016/Coding-Emoji   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
