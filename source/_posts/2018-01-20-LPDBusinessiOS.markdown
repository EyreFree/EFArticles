---
layout: post
title: 蜂鸟商家版 iOS 组件化 / 模块化实践总结
date: 2018-01-20 10:00:00 -05:00
categories: iOS
tag: Swift
---

![](https://user-gold-cdn.xitu.io/2018/1/19/1610f02b4af3f3d9?w=750&h=400&f=png&s=302366)

## 零. 前言

> “蜂鸟配送商家版”是一款针对商家打造的专业配送软件，有了这款应用，您可以使用蜂鸟商家版呼叫所有平台订单及电话订单配送，餐饮、鲜花、蛋糕、生鲜、商超均可配送。超低运费，清晰合理。海量补贴，充值返现。

以上这段对「蜂鸟商家版」的描述摘自 [蜂鸟配送官网](https://fengniao.ele.me/business.html)，大概可以理解为蜂鸟商家版是一个给广大商家用来发单呼叫配送员的 App。许多同学可能只听说过「饿了么」外卖应用，但是对支撑起外卖配送的后勤业务「蜂鸟配送」却知之甚少，实际上每天海量的外卖订单都是由蜂鸟配送系统进行处理和配送最终送到消费者手中的。外卖 O2O 是由外卖平台、商户、配送系统这三方合作共同完成的，缺一不可。O2O 最核心的价值就是人与服务的连接，而这种连接最终都是通过配送才得以实现的。

自 2016 年底开始我参与蜂鸟商家版的维护工作，除了日常的开发迭代以外，期间还参与推进了项目 Swift 化、项目组件化 / 模块化、非业务组件开源化等技术改造工作，今天这篇文章就给大家分享一下蜂鸟商家版 iOS 的组件化 / 模块化实践过程和自己的心得体会。

## 一. 背景分析

蜂鸟商家版 iOS 端代码使用 Git 进行管理，代码托管在内网的 GitLab 上。项目的依赖管理工具是大家比较熟悉的 CocoaPods，除了 RN 模块为了和 Android 组公用采用 Submodule 进行管理外，其他所有的子模块都采用 Pods 库的方式引入。

### 1. 存在的问题

在「蜂鸟商家版 iOS 组件化 / 模块化」工作开展之前，项目主要存在如下这些问题：

- 项目臃肿不堪

在组件化 / 模块化之前，蜂鸟商家版 App 的所有代码 / 资源文件等都是在同一个主工程里的，只有 RN 仓库或组内公用私有库等极少部分代码游离于主工程之外，所以在开发时，每一次都要编译整个项目的所有代码，十分低效。这个问题在独立开发时还不是十分明显，毕竟虽然项目大但是代码只有一个人在提交，所以项目代码量增加也不是那么夸张而且对项目发生的变化比较熟悉。但是当多人协作开发时，这个缺陷就暴露了出来，大家在各自开发不同的业务时，不仅要时刻和他人同步项目变化、读懂他人代码，还要每次编译完整个项目才能对自己所做的一点修改进行调试，效率低下。

- 团队规模变化

我开始参与蜂鸟商家版 iOS 端的维护时，之前只有一个前辈在维护，也就是一个人独立维护一个 App。然后过了没多久，他离职去了另一家公司，所以又变成了一个人独立维护这个 App。这时候因为是独立开发，所以也不存在什么太大的问题。但随着团队扩大，后面陆续来了几位同事共同负责这个项目的维护工作，大家都在同一个工程上进行业务开发，经常遇到如代码冲突、开发效率低下、职责划分不清、代码管理混乱等问题。

- 业务发展压力

由于公司处在高速发展的阶段，业务增长很快，最直观的表现就是市场 & 客服部门不断接到大量一线使用者的使用反馈或诉求，最后就变成了产品展示给我们开发人员的一份接一份的 PRD。紧凑的业务开发需求和各种灵活的功能迫使我们想尽一切能够使用的办法来提高开发效率，提高提测质量。

- 代码管理混乱

当我开始参与这个项目的维护时，这个项目就已经是一个 Swift 和 OC 混编的项目了，然后还有 RN 和 H5 代码，可以说是十分复杂了。虽然这不是我厂唯一一个 Swift 和 OC 的混编项目，但绝对是当时 Swift 化最高的一个项目，约 25% 的代码为 Swift。众所周知，Swift 和 OC 的互相调用远不如 Java 和 Kotlin 的互相调用那么顺滑（反正你现在知道了），并且处处藏着危机，暗坑无数，所以迫切需要找一个方式，将 Swift 和 OC 代码进行整理、转换或者分隔。毕竟，这个文件是 OC 下一个文件就是 Swift 这种频繁的思维转换在业务开发这种本就十分紧张的场景下，会使人十分疲惫，不利于开发工作的顺利进行。

![](https://user-gold-cdn.xitu.io/2018/1/19/1610f02b4ad93d93?w=800&h=683&f=png&s=230419)

### 2. 怎样去解决

为了解决以上这些问题，我们曾经进行过如下一些探索：

1. 移除无用的第三方库和资源文件，减少打包时间：效果不明显；
2. 整理并推动内部 Gitflow 工作流，提高协作效率：有一些效果，但由于项目过大，日常协作仍然吃力；
3. 研究 Swift 编译时间优化方法，提高编译效率：发现增加编译时间的都是 Swift 的一些常用语法糖，如果不用的话，严重降低开发效率，遂放弃；
4. 在不拆分主工程的情况下，推动项目整个 Swift 化：由于之前维护项目的前辈离职，导致目前的项目开发人员都对原代码不是十分熟悉，不敢妄加改动，加之业务迭代频繁，开发和测试资源都十分紧张，该工作工作推进十分缓慢。

可以发现上述尝试的结果都不是十分理想，在与 iOS 组内大佬们进行一些沟通，听取大佬们的意见后，决定对原项目进行「组件化 / 模块化拆分」工作，它能带来如下这些好处：

- 加快编译速度，不用再编译组件 / 模块外没有被依赖到的代码；
- 便于将每个模块指定给不同负责人进行管理；
- 降低合并难度，减小冲突和出错概率，提高业务开发效率；
- 将 Swift 和 OC 代码进行分离，便于进一步 Swift 化工作的推进；
- 可为模块编写单元测试，提高工作效率，同时方便测试人员进行有针对性的测试。

## 二. 目标设定

- 功能组件独立：保证所有的底层功能组件从主工程抽出，独立与主工程之外，便于复用、业务模块的调用；
- 业务模块划分与拆解：将业务按对应用途进行划分和拆解，想办法切断各业务之间的强依赖；
- 所有组件 / 模块独立编译：所有功能组件和业务模块能够独立于主工程进行编译，有各自的 Demo 工程；
- CocoaPods 发布：在内网 GitLab 进行发布，并且之后对每个模块用 GitFlow 工作流进行管理和后续发布工作。

![](https://user-gold-cdn.xitu.io/2018/1/19/1610f02b4ae5935d?w=840&h=351&f=png&s=110856)

## 三. 计划制定

说到组件化 / 模块化，那么什么是组件化 / 模块化呢？组件化和模块化的区别又在哪里呢？

组件，就是我们对功能的封装，一个功能就是一个组件，数据库、网络、文件操作、社会化分享等等这些功能都是组件。我们之所以要搞出组件的概念，是为了能够让我们的上层业务模块能够随时依赖和调用这些基础功能。组件基本上可以分为基础功能组件、通用 UI 组件、基础业务组件等这几类。所以为了满足上述要求，组件必须具有较高的独立性、扩展性以及复用性。

模块，就是对一系列有内聚性的业务进行整理，将其与其它业务进行切割、拆分，从主工程或原所在位置抽离为一个相对独立的部分。仅仅针对业务而言，比如说我们可以把订单业务独立为为一个模块，可以把个人中心独立为一个模块，把用户登录独立为一个模块等，在 App 中的体现就是一个个独立的 Git 仓库。模块化的一个好处是用到时可以搭积木，比如可以多个工程间复用同一个或几个业务模块，比如腾讯的 QQ 和 TIM，除了 UI 界面外 TIM 显然复用了大量现有的原 QQ 工程的业务模块代码，当然，我们这里暂时并没有这个需求。

经过小组会议讨论，我们的想法是将共用组件独立出来，然后直接按业务对现有主工程进行拆分同时兼顾 Swift 与 OC 分离，大致划分如下表所示：

### 1. 组件

| 组件 | 库名 | 主要内容 |
|:---|:----|:----|
|基础（OC）|LPDBOCFoundationGarbage| 基础的 OC 组件，各种零散的、混乱的视图、组件、控件、常量、OC 宏定义等，全放在这里，供上层调用。和他的库名一样，其本质就大概就是个垃圾桶。 |
|基础（Swift）|LPDBPublicModule| 基础的 Swift 组件，包含一些公用的 Swift 扩展，和模块间解耦的协议。 |
|网络（OC）|LPDBNetwork| 网络组件，对 AFNetworking 的浅层封装，同时包含了和网络相关的业务功能。 |
|...|...|...|

### 2. 模块

| 模块 | 库名 | 主要内容 |
|:---|:----|:----|
|历史（OC）|LPDBHistoryModule| 历史订单模块，包含和历史订单相关的资源文件、UI、业务逻辑代码等。|
|登录（OC）|LPDBLoginModule|用户登录模块，包含和登录、注册页面相关的资源文件、UI、业务逻辑代码等。|
|用户中心（OC）| LPDBUserCenterModule | 用户中心模块，包含和用户个人中心以及状态相关的资源文件、UI、业务逻辑代码等。|
|...|...|...|

### 3. 关系

按照上面的思路，理想化的模块 / 组件依赖关系图大概是这个样子的：

![](https://user-gold-cdn.xitu.io/2018/1/19/1610f02b4a9c953e?w=548&h=272&f=png&s=22346)

因为蜂鸟商家版的团队开发人员之前均没有过任何项目的拆分经验，大家也都是摸着石头过河，走一步看一步。所以虽然以上的拆分思路总体是对的，先拆组件后拆业务，但由于各种各样的原因，一些问题就在接下来的工作实施过程中暴露了出来。

## 四. 工作实施

我们小组主要还是以业务开发为主，所以组件化 / 模块化工作都是大家抽空闲时间来完成，并没有进行硬性的排期和设置 Deadline。按照之前制定的计划，我们进行了以下这些工作：

### 1. 功能组件独立

#### 1.1 LPDBOCFoundationGarbage

LPDBOCFoundationGarbage 是我们项目最先抽出的部分，这个库将和 LPDBPublicModule 一起，作为整个工程的最底层，再往下就是。这个库的定位和它的名字一样，就是一个垃圾桶，啥都往里放。其中大致包含以下一些东西：

- 自定义的 View 和控件，例如：小红点控件、刷新控件、加载控件、Tips 视图等；
- 自定义的 Controller，例如：基础控制器 BaseViewController、WebView 基础控制器 BaseWebViewController、自定义的弹框 AlertController等；
- 和业务相关的对基本类型或系统控件的扩展：对 NSObject、UIButton、UIImageView、UILabel 等添加的扩展代码 category；
- 甚至版本控制模块 LPDBVersionManager 也放在了这里。

因为我们在进行拆分任务的同时，还在同时维持着项目的开发工作，所以我们暂时没有精力做细致的拆分工作，只能先把这些零散的部分先放在一起进行管理。

#### 1.2 LPDBPublicModule

LPDBPublicModule 是基础的 Swift 组件，这个库主要包含：

- 一些公用的 Swift 扩展，例如：对 CGFloat、Date、NSString 等系统类型的 extension；
- 用于模块间解耦的协议。

因为工程内的 Swift 代码大多是我们新写的，所以相对旧的 OC 代码而言，整理地更好一些，所以这个仓库干净很多

#### 1.3 LPDBNetwork

LPDBNetwork 网络组件是我们项目完成 OC 和 Swift 基础部分后最先抽出的部分，刚开始我们认为这部分仅仅是单纯的业务网络请求操作和对 AFNetworking 的浅层封装，不包含界面 UI 逻辑等。不过当我们拆解完成后，发现其中还包含了一堆奇怪的东西：

- 对 AFNetworking 的封装和网络操作的一些定义，例如：LPDBHttpManager、LPDBRequestObject 和 LPDBModel 等；
- UI 操作，例如：等待视图 LPDBLoadingView 和 网络请求失败的提示等。

这一部分的话，因为都是比较古老的代码，所以当初的开发人员都已经不再继续维护了，所以在只能是我们自己进行拆分的情况下，为了防止大的变更导致发生问题，所以没有对这一块进行更细致的拆解工作。毕竟再烂代码也比不能工作的代码要好。

#### 1.4 LPDBUIKit

Swift 的 UI 库，我们将工程中的一些 Swift 视图和控件收集到了这个项目中，主要包含以下这些内容：

- 视图，例如：LPDBEmptyDataView、SlideScrollView 等；
- 控件，例如：SlideTabKit 等。

因为 Swift 代码总量还不是很大，所以这个库的东西目前也不是很多，以后会逐渐丰富起来。

### 2. 业务模块拆分

完成了上面的组件库的独立工作后，业务模块的拆解就相对轻松一些了，目前我们主要完成了三个业务模块的拆分工作。

#### 2.1 LPDBHistoryModule

LPDBHistoryModule 历史订单模块，和历史订单页面相关的信息都在该模块中，主要包含以下内容：

- UI，例如：历史订单界面、历史订单列表 Cell、加载视图等；
- 数据模型，例如：历史订单模型；
- 历史订单列表相关的网络请求。

因为该模块相对来说比较独立，所以拆分过程也比较顺利，主要依赖了 LPDBPublicModule、LPDBNetwork、LPDBOCFoundationGarbage 组件。

#### 2.2 LPDBLoginModule

LPDBLoginModule 用户登录模块是一个与用户登录、注册以及用户登录信息有关的模块，主要包含了以下信息：

- UI，例如：用户登录界面、用户注册界面等；
- 数据模型，例如：用户信息模型、用户信息地址模型等；
- 登录与注册相关的网络请求。

该模块相比较历史订单模块复杂了一些，不过仍然比较顺利，主要依赖了 LPDBPublicModule、LPDBOCFoundationGarbage、LPDBNetwork 组件。

#### 2.3 LPDBUserCenterModule

LPDBUserCenterModule 用户中心模块是一个与用户个人中心以及用户信息修改有关的模块，主要包含了以下信息：

- UI，例如：用户中心界面、用户电话修改界面、用户密码修改界面等；
- 数据模型，例如：用户详细信息模型、用户信息地址模型等；
- 用户中心相关的网络请求，例如：修改电话号码、请求验证码等。

该模块主要依赖了 LPDBOCFoundationGarbage 组件和 LPDBLoginModule 模块。

#### 2.4 其它

剩下的其他一些模块仍然处于计划中的状态，暂未进行拆分。到这一步的话，库间依赖关系大致如下图所示：

![](https://user-gold-cdn.xitu.io/2018/1/19/1610f02b4a8c0fca?w=1240&h=608&f=png&s=111714)

可以看到其中存在一些不太合理的依赖关系，如 LPDBUserCenterModule 依赖 LPDBLoginModule 模块，也就是所谓的业务模块横向依赖问题，接下来，我们就要处理这一问题。

### 3. 解除耦合

由于之前开发过程中从未有过任何模块化的考量，所以蜂鸟商家版的代码非常杂糅，项目依赖关系十分复杂，主要可以分为以下三类耦合：

- 界面耦合：App 执行过程中，硬编码的界面间的跳转行为；
- 工程耦合：某些模块在运行时需要依赖主工程的代码才能运行或实现完整的功能；
- 依赖耦合：两个业务模块之间的有依赖。

#### 3.1 模块间组件共用

在拆分业务模块的过程中，经常发生两个业务模块同时引用某一块业务代码的问题，这时我们就需要对这一块代码进行理解，首先区分它到底应不应该划分到业务层来？

- 如果是的话，应该划归到哪一个模块中去更合理一些；
- 如果不是的话，应该将这一部分代码下沉到哪一个组件库中去比较合适，或者独立为一个组件。

在 LPDBUserCenterModule 的抽离过程中就遇到了这个问题，LPDBUserCenterModule 
 和 LPDBLoginModule 共同依赖了几个和用户信息有关的数据模型，导致需要发生模块间横向依赖，所以我们将共用的数据模型抽出，然后下沉到了 LPDBOCFoundationGarbage 中。

#### 3.2 模块间耦合

另一个经常遇到的问题就是跨模块调用代码的问题了，不仅是模块与模块间代码的互相调用、模块间页面的跳转，还有模块反向调用主工程代码等问题，这个问题的解决我们分了三步：

- 反射调用

因为工程的复杂性和以前代码的不规范，导致我们在处理切割业务模块时比较痛苦，所以我们在刚开始抽出模块时采用了一种快速但不太安全的方式进行解耦，比如在 LPDBUserCenterModule 模块中需要调用主工程的 getMiddlePageVC 方法时，我们用了如下临时解决方案：

```objc
if ([[UIApplication sharedApplication].delegate respondsToSelector:@selector(getMiddlePageVC)]) {
    UIViewController *info = [[UIApplication sharedApplication].delegate performSelector:@selector(getMiddlePageVC)];

    ...
}
```

然后在主工程的 中实现这个接口：

```objc
// .h
@interface AppDelegate : UIResponder <UIApplicationDelegate>

...
// LPDBUserCenterModule
- (UIViewController *)getMiddlePageVC;

...

@end

// .m
@implementation AppDelegate

...

- (UIViewController *)getMiddlePageVC {
    ...

    return xxx;
}

...

@end
```

这一方案的优点就是灵活，利用 NSClassFromString、performSelector 等方式，能够快速解决各种耦合问题，瞬间切割出模块。但缺点也显而易见，字符串硬编码，维护成本大，去掉了编译器检查，容易翻车。

- 协议调用

所以自然而言地，当我们的某个业务模块的拆分工作基本定型时，我们就开始将第一步中的反射调用方式替换为协议的方式进行调用，比如当 LPDBLoginModule 模块需要调用主工程的 getCoordinate 方法时，示例如下：

```objc
id delegate = [[UIApplication sharedApplication] delegate];

if (![delegate conformsToProtocol:@protocol(AppDelegateProtocol)]) {
    return;
}
CLLocationCoordinate2D coordinate = [delegate coordinate];
```

然后在主工程中实现该方法：

```objc
// .h
#import "AppDelegate.h"

@import LPDBLoginModule;

@interface AppDelegate (Protocol)  <AppDelegateProtocol>

@end

// .m
@implementation AppDelegate (Protocol)

- (CLLocationCoordinate2D)getCoordinate {
    return self.coordinate;
}

@end
```

但是，样的改变并不能彻底解决所编写的模块间互相调用的代码缺乏编译器检查的问题，而仅仅是对调用方做了判断加上了容错，并不能在编译期就让开发人员察觉到问题，一定要进行测试才可以，所以这种方式也不是十分理想。

- Lotusoot 解耦工具

那么为了彻底解决问题，我们开发和引入了组件通信和工具 Lotusoot，调用方式有下列几种可供参考：

- 服务调用

```swift
let lotus = s(AccountLotus.self) 
let accountModule: AccountLotus = LotusootCoordinator.lotusoot(lotus: lotus) as! AccountLotus
accountModule.login(username: "admin", password: "wow") { (error) in
    print(error ?? "")
}
```

- 短链注册

```swift
let error: NSError? = LotusootRouter.register(route: "newproj://account/login") { (lotusootURL) in
    accountModule.showLoginVC(username: "admin", password: "wow")
}
```

- 短链调用

```swift
let param: Dictionary = ["username" : "admin",
                                 "password" : "wow"]

// 无回调                                 
LotusootRouter.open(route: "newproj://account/login", params: param)
// 有回调
LotusootRouter.open(route: "newproj://account/login", params: param).completion { (error) in
    print(error ?? "open success")
}
// ⚠️不推荐的用法，用 ?pram0=xxx 这样的形式导致字符串散落在各处，不易管理。
// 但为了保证 Hybrid 项目中 H5 页面的正常跳转，提供了此种调用
LotusootRouter.open(url: "newproj://account/login?username=zhoulingyu").completion { (error) in
    print(error ?? "open success")
}
```

具体可以参见 [iOS 灵活的 模块化/组件化 工具与规范 Lotusoot 解说](http://zhoulingyu.com/2017/11/29/iOS-modularized-tool-Lotusoot/) 一文，在此不多做赘述。类似的工具还有 [BeeHive](https://github.com/alibaba/BeeHive) 和 [LPDMvvmRouterKit](https://github.com/LPD-iOS/LPDMvvmRouterKit) 等，大家可以自行进一步探索。

最终结构就变成了如图所示的样子：

![](https://user-gold-cdn.xitu.io/2018/1/19/1610f02b4aa60211?w=708&h=408&f=png&s=44455)


## 五. 问题整理

### 1. 不合理的分层结构和库间依赖

由于参与拆分工作的人员比较缺乏组件化经验，所以导致某些库的拆分不是十分合理，某些应该沉入底层的公用 Model 和常量等没有在开始时就放到一个合理的位置。业务模块之间也存在一些不合理的横向依赖，没有进行一个合理的业务边界划分。这些原因导致我们在进行拆分工作时经常需要回过头来对已经拆出来的模块和组件重新进行整理和处理，重复劳动量很大。

### 2. 拆分粒度不适中

某些库比如 LPDBOCFoundationGarbage 比较庞大，而像 LPDBUIKit 这样的库中内容却非常少，这一点的处理上存在问题。如果一个拆分完成的库仍然比较臃肿的化，说明仍然存在细化拆分的必余地。

### 3. 工作进度难以控制

由于没有能提前制定好详细的进度计划表，加上业务工作的挤压，导致我们花在组件化 / 模块化工作上的时间比较零散。本意是希望大家能够灵活安排工作，合理处置业务开发与技术改造工作之间的关系，但效果不是很理想，表现就是组件化 / 模块化工作的进行没有连续性，大家的积极性和工作效率也都不高。

## 六. 经验总结

### 1. 工作开始前要进行技术调研

查看和学习一些同类成功的案例资料或者向业内大佬们请教能够对计划的制定带来便利，能够使我们避免很多错误的设计，少走一些弯路，降低返工率。

### 2. 制定详细整体规划

> 在准备作战时，我常常发现定好的计划没有用处，但计划的过程仍必不可少。—— 德怀特·艾森豪威尔

制定详细的整体规划能够在设计阶段就将一些不合理的地方暴露出来，从而拿出解决方案使问题提前得到解决，或者把不合理的内容删减替换掉，例如分层不合理、库间依赖这样的问题，就会减少很多。拿出细致的任务拆分计划和工作量预估，也能更合理地将任务安排到开发人员手中，在提升工作效率的同时也能尽量避免和业务开发产生冲突。

### 3. 注意对代码质量的控制

好的代码和编码习惯能够大幅提升项目的可维护性，为之后的工作带来便利。我们之前旧的 OC 代码比较混乱，基本处于无法维护的状态，拆分起来十分痛苦；而新写的 Swift 代码明显质量要高很多（这真的不是我们自夸...），拆分起来就顺利多了。

### 4. 重视信息的文档化

每一个拆分出的模块及时添加文档，嫌麻烦的话至少要建立一份通用的 README 模板，每一个模块或组件的建立者把模块内容、拆分目的、设计思路等基本信息记录一下，有什么坑或者注意点也可以文档化，是以后的长期项目维护成为可能。

## 七. 开源成果

我们在组件化 / 模块化工作期间，产出的一些库和工具放在了 GitHub 上进行开源，给大家一些借鉴的同时，也希望能够收到大家的意见和建议，提高我们项目本身的质量：

| 库名 | 简介 | 仓库地址 |
|:-----|:-----|:---------|
| Lotusoot | 灵活的 Swift 组件解耦和通信工具 | [https://github.com/Vegetarians/Lotusoot](https://github.com/Vegetarians/Lotusoot) |
| Bamboots | 一个面向协议的 Swift 网络库 | [https://github.com/mmoaay/Bamboots](https://github.com/mmoaay/Bamboots) |
| bigkeeper | 一个 iOS & Android 模块化项目效率提升工具 | [https://github.com/BigKeeper/bigkeeper](https://github.com/BigKeeper/bigkeeper) |
| SideNavigation | 一个支持侧滑且可自定义的侧边栏 | [https://github.com/CNKCQ/SideNavigation](https://github.com/CNKCQ/SideNavigation) |
| ViewPagers | 一个支持手势的 Segmented Control | [https://github.com/CNKCQ/ViewPagers](https://github.com/CNKCQ/ViewPagers) |
| EFAutoScrollLabel | 一个带跑马灯效果的 UILabel | [https://github.com/EyreFree/EFAutoScrollLabel](https://github.com/EyreFree/EFAutoScrollLabel) |

## 八. 后记

本文基本描述了蜂鸟商家版 App 到目前为止的组件化 / 模块化实践情况，希望本文能够给您的移动项目演进提供一些借鉴。在此过程中我们产出的一些文章、开源库和工具，也希望能给大家带来一定的帮助或者启发。欢迎大家提出各种反馈和建议或，帮助我们继续改进和提高。

2017 年底，也就是差不多我参与蜂鸟商家版的维护工作满一年的样子，由于业务调整的原因这个 App 已经移交给别的团队进行维护了，导致项目的 Swift 化和组件化 / 模块化工作并没有全部完成，这一点有些遗憾。不过还是希望蜂鸟商家版能够越来越好，继续为广大商家朋友们服务。

好消息是，接下来我主要参与蜂鸟团队版 App 的架构工作，这一次我们根据之前暴露出的问题制定了详细的工作计划，有了蜂鸟商家版的踩坑经验后，我相信这一次我们一定能顺利完成目标。2018，加油，一起拼！

本文编写过程中参考了以下文章，在此对原作者们表示感谢：

1. [即时配送网之于外卖O2O，配送的更高境界是社群经营](https://36kr.com/p/5100487.html)
2. [谈谈我的理解-组件化/模块化](https://www.jianshu.com/p/79e4df63f31f)
3. [蘑菇街 App 的组件化之路](http://limboy.me/tech/2016/03/10/mgj-components.html)
4. [豆瓣App的模块化实践](http://lincode.github.io/Modularity)
5. [手机天猫解耦之路](http://www.infoq.com/cn/articles/the-road-of-mobile-tmall-decoupling)
6. [京东iOS客户端组件管理实践](http://www.infoq.com/cn/articles/jd-ios-component-management)

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://juejin.im/post/5a620cf5f265da3e36415764   
> 转载请注明原作者及以上信息。
