---
layout: post
title: Flutter 与 Swift 混编项目启用 Bitcode
date: 2020-11-17 10:00:00 -05:00
categories: iOS
tag: Swift
---

因为手头在做的项目是 Flutter 与 Swift 混编的，然后之前是已经启用 Bitcode 的，刚接入 Flutter 的时候，我把 Bitcode 的各种设置都启用了，比如：

1. 将各个 target 的 `Enable Bitcode` 选项设为 `Yes`；
2. 在 `Podfile` 尾部添加如下代码将 pod 的 `Enable Bitcode` 打开：

```ruby
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['ENABLE_BITCODE'] = 'YES'
    end
  end
end
```

但 Archive 时会报错，类似如下的错误输出：

```bash
bitcode bundle could not be generated because '/.../.ios/Flutter/engine/Flutter.framework/Flutter' was built without full bitcode. All frameworks and dylibs for bitcode must be generated from Xcode Archive or Install build file '/.../.ios/Flutter/engine/Flutter.framework/Flutter' for architecture armv7
```

找了一圈得到的回复基本都是 `修改 pod 库的 ENABLE_BITCODE = NO（因为 Flutter 现在不支持 bitcode）`，所以暂时关闭了。

但最近包体积膨胀得厉害，所以又回头来找找解决方案，毕竟 Flutter 官方是号称支持 Bitcode 的，而且还一本正经地搞了一个 [Creating an iOS Bitcode enabled app](https://github.com/flutter/flutter/wiki/Creating-an-iOS-Bitcode-enabled-app) 文档。

果然最后在 issue 里翻到了 [解决方案](https://github.com/flutter/flutter/issues/48092#issuecomment-577345215)：

在 Archive 之前终端切换到 flutter 模块下执行一次 `flutter build ios --release` 然后再 Archive 就可以了，已成功提交审核并上架 App Store，Adhoc 包也没问题。

![想不到吧](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1605613608337&di=c1eb216cb8a9c37b0ed37b622cb7249c&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201703%2F07%2F20170307134555_jtF2H.thumb.700_0.jpeg)

顺便贴一下我的 flutter doctor 输出：

```bash
[✓] Flutter (Channel unknown, 1.22.3, on Mac OS X 10.15.5 19F96, locale
    zh-Hans-CN)
    • Flutter version 1.22.3 at /Users/eyrefree/Documents/Tools/flutter
    • Framework revision 8874f21e79 (3 weeks ago), 2020-10-29 14:14:35 -0700
    • Engine revision a1440ca392
    • Dart version 2.10.3
    • Pub download mirror https://pub.flutter-io.cn
    • Flutter download mirror https://storage.flutter-io.cn

[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.0)
    • Android SDK at /Users/eyrefree/Library/Android/sdk
    • Platform android-30, build-tools 30.0.0
    • ANDROID_HOME = /Users/eyrefree/Library/Android/sdk
    • Java binary at: /Applications/Android
      Studio.app/Contents/jre/jdk/Contents/Home/bin/java
    • Java version OpenJDK Runtime Environment (build
      1.8.0_242-release-1644-b3-6222593)
    • All Android licenses accepted.

[✓] Xcode - develop for iOS and macOS (Xcode 12.2)
    • Xcode at /Applications/Xcode.app/Contents/Developer
    • Xcode 12.2, Build version 12B45b
    • CocoaPods version 1.10.0

[✓] Android Studio (version 4.0)
    • Android Studio at /Applications/Android Studio.app/Contents
    • Flutter plugin version 46.0.2
    • Dart plugin version 193.7361
    • Java version OpenJDK Runtime Environment (build
      1.8.0_242-release-1644-b3-6222593)

[✓] VS Code (version 1.51.0)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.16.0
```

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://www.eyrefree.org/2020/11/17/Flutter-Bitcode   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
