---
layout: post
title: Swift UIColor 添加从十六进制值初始化的扩展
date: 2015-09-10 10:00:00 -05:00
categories: iOS
tag: Swift
---

在实际开发中，我们拿到的设计图上的颜色往往标注的是十六进制的，而在不添加第三方库的情况下 UIColor 并不支持从十六进制数字初始化，手动将十六进制颜色转化为 RGB 形式十分浪费精力，我们可以通过为 UIColor 添加扩展的方式来支持直接从十六进制数值初始化从而为我们的开发带来便利。

---
# 1.添加文件
在项目中添加一个用于编写扩展代码的文件，将其命名为 `UIColor+valueRGB.swift`。

# 2.添加扩展代码
在第一步创建的文件中添加如下代码：
```swift
import UIKit

extension UIColor {
    //用数值初始化颜色，便于生成设计图上标明的十六进制颜色
    convenience init(valueRGB: UInt) {
        self.init(
            red: CGFloat((valueRGB & 0xFF0000) >> 16) / 255.0,
            green: CGFloat((valueRGB & 0x00FF00) >> 8) / 255.0,
            blue: CGFloat(valueRGB & 0x0000FF) / 255.0,
            alpha: CGFloat(1.0)
        )
    }
}
```

# 3.调用
然后我们就可以在工程中以如下方式直接从十六进制数字初始化 UIColor 了：
```swift
let testColor = UIColor(valueRGB: 0x666666)
```

---
实际上这里没有做十六进制的限定，只需要是 UInt 类型都可以，但是貌似暂时没发现什么实际意义，用来生成随机颜色？或者是画颜色表？大家可以自行挖掘下，😊

---
本文链接：[http://www.eyrefree.org/2015/09/10/2015-09-10-Swift-UIColor-Hex/](http://www.eyrefree.org/2015/09/10/2015-09-10-Swift-UIColor-Hex/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
