---
layout: post
title: iOS 在 App 中使用自定义字体
date: 2017-03-23 10:00:00 -05:00
categories: iOS
tag: TTF
---

最近在做一个神奇的 App 需要添加楷体，检查了一下发现 iOS 默认并不会安装这种字体，需要我们自己将字体文件添加到 App 中，本文主要记录了添加自定义字体的过程、添加完成后的效果以及遇到的一些坑，文中 iOS 代码主要为 Swift 3。

---

# 1. 查看全部可用字体

在进行操作之前，我们先查看默认情况下，系统的可用字体有哪些，利用如下代码可以将系统全部字体的 FontFamilyName 以及它们的 FontName 进行打印：

```swift
for fontFamily in UIFont.familyNames {
    print(fontFamily)

    for font in UIFont.fontNames(forFamilyName: fontFamily) {
        print(fontFamily + ": " + font)
    }
}
```

我们可以在日志输出窗口搜索我们需要的楷体，可以看到默认并没有安装，效果如图所示：

![查看全部可用字体](http://upload-images.jianshu.io/upload_images/1018190-7350c33cc8513393.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 2. 获取字体文件

首先，我们需要获取字体文件，一般文件类型为 ttf 或 ttc 的就是字体文件了，如图所示：

![字体文件](http://upload-images.jianshu.io/upload_images/1018190-8128dda0b2489f39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以在 [字体口袋](http://www.zitikoudai.com/)，[搜字网](http://www.sozi.cn/) 之类的网站找到很多可供下载的资源：

![字体口袋](http://upload-images.jianshu.io/upload_images/1018190-510dc0d1628c7b4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

或者也可以在 OS X 的系统字体册找到我们想要的字体，可以从应用程序列表中打开字体册：

![字体册](http://upload-images.jianshu.io/upload_images/1018190-ec156aacb9194153.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择 `所有字体` 然后在搜索栏内键入需要查找的字体名即可列出匹配的项目：

![在字体册中查找字体](http://upload-images.jianshu.io/upload_images/1018190-29901f9b4632925f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

右键点击想要的字体选择 `在 Finder 中显示` 即可找到对应的字体文件。

# 3. 添加字体文件到工程

将我们获取的字体文件直接拖到工程中的合适位置，如图所示：

![添加字体文件](http://upload-images.jianshu.io/upload_images/1018190-2aad3e8abae38ee9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加完成后选中对应的字体文件可进行预览：

![预览](http://upload-images.jianshu.io/upload_images/1018190-5f5b457a8bdda98b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们还需要在 `Info.plist` 文件中添加 Fonts provided by application 项，如图所示：

![Info.plist 添加 Fonts provided by application 项](http://upload-images.jianshu.io/upload_images/1018190-be03386c8a892c1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可通过直接添加代码的方式完成，例如这里添加两个字体文件 STKaiti.ttf 和 Kaiti-SC.ttf 的代码如下：

```
<key>UIAppFonts</key>
<array>
    <string>STKaiti.ttf</string>
    <string>Kaiti-SC.ttf</string>
</array>
```

这时，我们对工程进行编译，再次查看可用的全部字体，这时我们可以看到，我们需要的楷体已经添加了进来：

![成功添加楷体](http://upload-images.jianshu.io/upload_images/1018190-35090eacafa8f523.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 4. 字体的使用

## 1. StoryBoard

在 StoryBoard 中使用的话，只需要设置控件的 Font 属性为，选择 Custom，然后再从 Family 中选择需要的字体即可。

![在 StoryBoard 中使用](http://upload-images.jianshu.io/upload_images/1018190-ae1d2f52ed178db2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2. 代码

我们直接通过如下代码直接生成一个楷体的字体对象，将其赋给 UIButton 或者 UILabel 等空间对应的属性即可。

```swift
UIFont(name: "STKaiti", size: 20)
```

这里需要注意的是 UIFont 的 name 字符串必须是上面我们打印出的字体名称，和字体文件的文件名或者其他信息无关。如果这里我们输入了一个无效的字体名称，可能会返回一个空的对象，所以我的使用方式如下：

```swift
import Foundation

extension UIFont {

    static func boldKaiti(ofSize fontSize: CGFloat) -> UIFont {
        return UIFont(name: "Kaiti SC Black", size: fontSize) ?? UIFont.systemFont(ofSize: fontSize)
    }

    static func kaiti(ofSize fontSize: CGFloat) -> UIFont {
        return UIFont(name: "Kaiti SC", size: fontSize) ?? UIFont.systemFont(ofSize: fontSize)
    }
}
```

使用楷体前后效果对比，可以看到换个字体以后感觉整个 feel 就不一样了，可见我们要好好听设计师蜀黍们的话，该用啥字体用啥字体，不能偷懒，😂 （嘛，控件位置还没调整，第二段可能有点放不下了）：

添加字体前|添加字体后
:-------------------------:|:-------------------------:
![](http://upload-images.jianshu.io/upload_images/1018190-af181721cca8c174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1018190-e089745639153fd7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5. 一些坑

#### 1. 字体文件过大

如果你用的字体文件是 TTC 格式的，可以考虑去下载单独的 TTF 字体文件，TTC 是几个 TTF 合成的字库，里面包含不止一种字体类型。

然后多个类似的字体，可以和设计师商量一下统一使用同一种字体。

唔，如果是单个 TTF 文件过大的话，暂时木有找到好的解决办法，可以考虑多下几个不同来源的同种字体的文件，挑一个体积最小的。或者对现有的 TTF 文件进行编辑，将一些低频字符进行删除。

#### 2. 字体重名问题

在导入同一种字体的不同风格时，比如这里楷体的粗体 `Kaiti-SC-Black` 和普通体 `Kaiti-SC-Regular` ，在 App 中打印出的 FontName 居然只有一个楷体的，这是为啥呢，推测可能是字体文件生成的时候填写字体名偷工减料，没有填写完整的字体名或者字体名识别异常导致的。

![只有一个楷体](http://upload-images.jianshu.io/upload_images/1018190-cd76f9fd80856fbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我找了一个 OS X 下可用的免费字体编辑工具 BirdFont 对字体文件进行查看想一探究竟，官网地址 [https://birdfont.org/](https://birdfont.org/)，我用的是 [2.15.5](http://eyrefree.coding.me/FileKeeper/birdfont-2.15.5-free.dmg) 版本，大家可以自行去官网下载最新版。

在 Finder 中打开我们的字体文件，右键选择用 BirdFont 进行打开即可，因为字体文件数据量较大，打开过程可能会有些长，需要耐心等待几分钟，具体时长根据数据量而定，等软件右上角的 Loading 消失即表示打开完成。

点击右上角菜单，选择 Name and Description 选项可打开字体描述信息编辑页面：

![Name and Description](http://upload-images.jianshu.io/upload_images/1018190-f0a8629b4d2b6382.png)

在这里我们可以看到，Kaiti-SC-Black 和 Kaiti-SC-Regular 两个字体文件的 `Name` 一栏确实是只写了 Kaiti SC，和我们之前在 App 中输出的字体名称一致，`Style` 一栏虽然有所区别，但是我们在 App 中是无法通过 `Style` 这个参数来找到某个字体的（反正我没找到，如果真的有办法希望可以教我，蟹蟹，😂 ），所以这应该就是我们只能在 App 中找到一个楷体的原因了。

BirdFont Kaiti-SC-Black|BirdFont Kaiti-SC-Regular|App
:-------------------------:|:-------------------------:|:-------------------------:
![](http://upload-images.jianshu.io/upload_images/1018190-36d14efc746d886a.png)|![](http://upload-images.jianshu.io/upload_images/1018190-c828d1f366a82258.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1018190-4b11a9f1122e702b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们对其中一个字体的 `Name` 做一下修改，反正使俩字体文件的 Name 不一样就行，然后我这里将 Kaiti-SC-Black 的 `Name` 改为 Kaiti SC Black，改完之后需要先 Save，然后选择 Import and Export：

![Import and Export](http://upload-images.jianshu.io/upload_images/1018190-c27be693b5abdae5.png)

然后再选择 Export Fonts：

![Export Fonts](http://upload-images.jianshu.io/upload_images/1018190-0d4f161f80baa28a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后会弹出 Export Settings 页面进行一些参数设置，注意将 Formats 中的 TTF 选项勾选即可，其他的两个选项可以去掉，加快导出速度。

![Export Settings](http://upload-images.jianshu.io/upload_images/1018190-2a2a4271cfbc9133.png)

然后单击下面的 Export 按钮即可开始导出工作，右上角会出现一个 Loading 视图，等它消失就表示导出完成了，导出完成后会在 Finder 中打开对应字体文件。

![导出完成](http://upload-images.jianshu.io/upload_images/1018190-06962a1c5acf94a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们将其添加到工程中再看下能不能找到它：

![新增 Kaiti SC Black 字体](http://upload-images.jianshu.io/upload_images/1018190-96d28c83f011ed0c.png)

可以看到这一次多了一个名为 Kaiti SC Black 的字体，完成！

PS：

最后吐槽一下，BirdFont 这工具真的好慢，巨慢，慢到爆炸，🙄 。大家在操作过过程中尽量挑体积小一点的字体文件进行操作。不过还好，使用过程中还没遇到闪退之类的状况，功能上没问题。希望后续版本能够提高处理速度。

---

本文链接：[http://www.eyrefree.org/2017/03/23/2017-03-23-UIFont-TTF/](http://www.eyrefree.org/2017/03/23/2017-03-23-UIFont-TTF/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
