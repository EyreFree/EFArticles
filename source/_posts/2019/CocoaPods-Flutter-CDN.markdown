---
layout: post
title: CocoaPods installed but not initialized. Skipping pod install
date: 2019-10-17 10:00:00 -05:00
categories: Code
tag: CocoaPods
---

流水账警告，⚠️

---

CocoaPods [1.8.0](http://blog.cocoapods.org/CocoaPods-1.8.0-beta/) 支持了 CDN，速度比以前快了很多，这么棒当然是赶紧用起来了，于是推动组内升到了 1.8.3 并移除了无用的 repo，然后 Jenkins 打包就炸了，发现报错：

```
/Users/XXX/Desktop/jenkinsWorkspace/workspace/x_iOS/XXX/XXX-Bridging-Header.h:60:9: 'FlutterPluginRegistrant/GeneratedPluginRegistrant.h' file not found
```

唔，看起来是依赖的 Flutter 模块炸了，去 Flutter 的打包日志看看，找到如下报错：

```
Build...
Warning: Building for device with codesigning disabled. You will have to manually codesign before deploying to device.
Building com.x w s.XXX for device (ios-release)...
Warning: CocoaPods installed but not initialized. Skipping pod install.
  CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
  Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
  For more info, see https://flutter.io/platform-plugins
To initialize CocoaPods, run:
  pod setup
once to finalize CocoaPods' installation.
Running Xcode build...                                          
Xcode build done.                                           57.4s
Failed to build iOS app
Error output from Xcode build:
↳
    ** BUILD FAILED **
```

噫，`pod install` 被跳过了？切到打包机执行了 `flutter doctor` 命令，查看一下是不是 flutter 环境坏掉了：

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v1.3.8, on Mac OS X 10.14.6 18G95, locale zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
[!] iOS toolchain - develop for iOS devices (Xcode 11.1)
    ✗ CocoaPods installed but not initialized.
        CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
        For more info, see https://flutter.io/platform-plugins
      To initialize CocoaPods, run:
        pod setup
      once to finalize CocoaPods' installation.
[✓] Android Studio (version 3.2)
[!] Connected device
    ! No devices available
```

唔，出现了一样的报错，看起来是升级 CocoaPods 导致的异常。按日志的提示我们执行了 `pod setup`，操作成功！ `flutter doctor` 报错依旧。咦，怎么肥事？经过 [一番搜索](https://github.com/flutter/flutter/issues/41291) 发现，CocoaPods 1.8.x 以下在 setup 时初始化的 repo 是 `master`，而 1.8.x 已经不再是这个了：

![](/images/2019/CocoaPods-Flutter-CDN/1.png)

但不知道是 flutter 还是 CocoaPods 的问题（可能是因为我们用的 flutter 版本低了，是 1.3.8），打包过程会通过检测旧版本的主 repo 是否存在来判断 CocoaPods 是否已经初始化，所以我们需要先切换到 1.7.5，然后执行 `pod setup`，再切回 1.8.3 即可，`flutter doctor` 显示正常，打包也恢复正常了。

```
sudo gem uninstall cocoapods
sudo gem install cocoapods -v 1.7.5
pod setup
```

然后再安装最新版 CocoaPods 就可以了：

```
sudo gem install cocoapods
```

提示：记住这里千万不要删除多余的 repo，不然你就会像我一样需要白忙活 2 个小时，首尾呼应，完结撒花。

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://www.eyrefree.org/2019/CocoaPods-Flutter-CDN   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
