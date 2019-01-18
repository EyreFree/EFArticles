---
layout: post
title: iOS 为 App 添加 3D Touch 快捷菜单
date: 2016-09-22 10:00:00 -05:00
categories: iOS
tag: 3D Touch
---

iOS 为 App 图标添加 3D Touch 快捷启动菜单，Demo 地址：  
[https://github.com/EyreFree/EF3DTouchDemo](https://github.com/EyreFree/EF3DTouchDemo)

---

# 1.注意事项

3D Touch 只在 iOS 9 及以上版本得到支持，之前版本的 iOS 并不支持该功能；
3D Touch 只在 iPhone 6s 及以后型号的 iPhone 或 iPad Pro 上可用，更早的设备并不支持该功能。
具体可通过如下代码进行判断：

```
if self.traitCollection.forceTouchCapability == UIForceTouchCapability.available {
    print("支持 3D Touch")
} else {
    print("不支持 3D Touch")
}
```

# 2.添加按钮

右键点击工程中的 Info.plist 文件选择打开方式为 Source Code：

<center>
![以 Source Code 方式打开 Info.plist](/images/3D-Touch-1.png)
</center>

在其中填写如下代码：

```
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShuffle</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>3D Touch 测试按钮</string>
        <key>UIApplicationShortcutItemType</key>
        <string>0</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeLove</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>出来吧，小火龙！</string>
        <key>UIApplicationShortcutItemType</key>
        <string>1</string>
    </dict>
</array>
```

其中 UIApplicationShortcutItemIconType 项代表按钮图标，更多图标可以参见： [https://developer.xamarin.com/api/type/UIKit.UIApplicationShortcutIconType/](https://developer.xamarin.com/api/type/UIKit.UIApplicationShortcutIconType/)

<center>
![在 Info.plist 添加按钮代码](/images/3D-Touch-2.png)
</center>

这段代码添加了两个 3D Touch 按钮，“3D Touch 测试按钮”和“3D 出来吧，小火龙！”。

<center>
![成功添加 3D Touch 按钮](/images/3D-Touch-3.png)
</center>

# 3.添加功能代码

打开 AppDelegate.swift 在其中添加如下代码，这段代码对点击按钮操作进行了处理，点击按钮后会进入 App 弹出一个显示按钮名称的对话框：

```
func application(_ application: UIApplication, performActionFor shortcutItem: UIApplicationShortcutItem, completionHandler: @escaping (Bool) -> Void) {

    var sourceButtonTitle: String?

    //根据按钮标题进行进一步操作
    switch shortcutItem.localizedTitle {
    case "3D Touch 测试按钮":
        sourceButtonTitle = "来源按钮：3D Touch 测试按钮"
        break
    case "出来吧，小火龙！":
        sourceButtonTitle = "来源按钮：出来吧，小火龙！"
        break
    default:
        break
    }

    //测试操作：弹出一个对话框显示来源按钮
    if let trySourceButtonTitle = sourceButtonTitle {
        let alert = UIAlertController(title: nil, message: trySourceButtonTitle, preferredStyle: .alert)
        alert.addAction(UIAlertAction(title: "知道啦", style: .cancel, handler: nil))
        self.window?.rootViewController?.present(alert, animated: true, completion: nil)
    }
}
```

<center>
![在 AppDelegate.swift 添加功能代码](/images/3D-Touch-4.png)
</center>

我们可以在这里添加代码从而实现根据不同来源按钮而执行不同的操作，结果如图所示：

<center>
![操作结果](/images/3D-Touch-5.png)
</center>

# 4.其他

1.需要注意的是，快捷启动按钮最多只能添加 4 个。
2.最新的 iOS 10 系统会给所有的 App 额外添加一个 3D Touch 分享按钮，点击后不打开 App 而是调用系统分享该应用的 App Store 下载地址。

---

参考资料：

[http://iostuts.io/2015/10/08/how-to-add-quick-actions/](http://iostuts.io/2015/10/08/how-to-add-quick-actions/)
[http://stackoverflow.com/questions/36369058/how-to-check-3d-touch-available-in-iphone-programatically](http://stackoverflow.com/questions/36369058/how-to-check-3d-touch-available-in-iphone-programatically)

---

本文链接：[http://www.eyrefree.org/2016/09/22/2016-09-22-3D-Touch/](http://www.eyrefree.org/2016/09/22/2016-09-22-3D-Touch/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)