---
layout: post
title: 利用 Travis-CI 让你在 GitHub 上的 CocoaPods 库持续构建
date: 2017-03-16 10:00:00 -05:00
categories: GitHub
tag: Travis-CI
---

Travis-CI 是一个专门为开源项目打造的持续集成环境，目前已经支持绝大部分主流语言，它采用 yaml 格式，简洁清新独树一帜（感谢百度百科，2333）。

每次 Commit 后会执行构建操作，并在 GitHub 对应的 Commit 后显示构建状态或结果，如图所示：

![持续构建效果预览](http://upload-images.jianshu.io/upload_images/1018190-87b43c1d2d1e9c1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

本文以 [EFQRCode](https://github.com/EyreFree/EFQRCode)(一个使用 Swift 作为开发语言的 CocoaPods 开源库) 为例，简述怎样为自己的开源项目添加持续构建功能。

# 1. 指定 Swift 版本

在根目录下添加一个 .swift-version 文件，在其中填写 Swift 版本号，例如这里 EFQRCode 库使用 Swift 3.0 进行开发，所以这里填写的是：

```
3.0
```

# 2. 添加 Travis-CI 配置文件

在根目录下添加一个 .travis.yml 文件，在其中填写配置信息：

```
osx_image: xcode8
language: objective-c

cache: cocoapods
podfile: Example/Podfile

env:
  global:
    - LANG=en_US.UTF-8
    - LC_ALL=en_US.UTF-8
    - XCODE_WORKSPACE=Example/EFQRCode.xcworkspace
  matrix:
    - SCHEME="EFQRCode-Example"

before_install:
  - gem install xcpretty --no-rdoc --no-ri --no-document --quiet
  - gem install cocoapods --pre --no-rdoc --no-ri --no-document --quiet
  - pod install --project-directory=Example

script:
  - set -o pipefail
  - xcodebuild -workspace "$XCODE_WORKSPACE" -scheme "$SCHEME" -configuration Debug clean build CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO | xcpretty -c
  - xcodebuild -workspace "$XCODE_WORKSPACE" -scheme "$SCHEME" -configuration Release clean build CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO | xcpretty -c
  - pod lib lint --no-clean

after_success:
  - sleep 5
```

# 3. 注册 Travis-CI 账号

打开 [https://travis-ci.org/](https://travis-ci.org/) 注册一个 Travis-CI 账号，也可以通过 GitHub 账户直接登陆。Travis-CI 服务对开源项目是免费的，所以你的私有项目无法享受到免费的持续构建服务。

# 4. 从 GitHub 同步项目

第一次进入时会自动从 GitHub 同步项目数据，可能需要等待一段的时间进行同步，同步完成后可以看到如下的项目列表：

![项目列表](http://upload-images.jianshu.io/upload_images/1018190-d01facdae4cb29f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一般情况下每隔一定的时间 Travis-CI 都会从 GitHub 自动同步数据，如果新添加的项目想要立刻同步到 Travis-CI 的话，可以手动点击右上角的 Sync account 同步按钮，如图所示：

![同步按钮](http://upload-images.jianshu.io/upload_images/1018190-d14d4450f3790330.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5. 开启持续集成

然后接下来就是开启对应项目的持续构建，大家应该已经猜到该怎么做了吧...将对应项目之前的 Switch 按钮设为启用绿色勾选状态即可，如图所示：

![勾选状态](http://upload-images.jianshu.io/upload_images/1018190-2085dfac1d55e776.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 6. 观察错误日志

若发生构建失败，可通过查看错误日志的方式来定位具体问题原因，可点击工程名，选择出错的那一次构建即可：

![构建日志](http://upload-images.jianshu.io/upload_images/1018190-a483be7d32c674bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 7. 一些废话

本文只提供了针对 Swift CocoaPods 库的操作步骤，Travis-CI 具体到每种语言／项目的构建配置各不相同，参数各异，有的时候还需要根据自己的项目特性做一些个性化的调整，需要我们多思考，多调试，多尝试，总之不要轻易放弃哇。别问我是怎么知道的，😂 ：

![坑](http://upload-images.jianshu.io/upload_images/1018190-7cbf867d9314dbbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

本文链接：[http://www.eyrefree.org/2017/03/16/2017-03-16-Travis-CI/](http://www.eyrefree.org/2017/03/16/2017-03-16-Travis-CI/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
