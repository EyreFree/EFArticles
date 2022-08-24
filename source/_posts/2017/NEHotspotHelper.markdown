---
layout: post
title: iOS 利用 NEHotspotHelper 获取 WiFi 列表
date: 2017-03-09 10:00:00 -05:00
categories: Code
tag: NEHotspotHelper
---

iOS 9 发布之后，苹果推出了 NetworkExtension，利用这个框架可以实现很多和网络相关的操作。本文主要介绍怎样使用其中的 NEHotspotHelper 进行设备 WiFi 列表的获取。

Demo 地址：[https://github.com/EyreFree/EFNEHotspotHelperDemo](https://github.com/EyreFree/EFNEHotspotHelperDemo)

# 一. 注意事项

1. 首先，NEHotspotHelper 只在 iOS 9 及以上版本得到支持，之前版本的 iOS 并不支持该功能；
2. 然后，你需要有一个开发者账号；
3. 最后，该框架目前还没有大规模开放使用，所以需要向苹果发送申请并且审核通过才能够获得使用该框架的权限，大致内容就是描述一下你需要使用该框架的原因之类的，然后我是用的英文进行描述（感谢百度以及谷歌翻译），不过据说中文也行。提交申请后大概一周内会收到反馈邮件，申请地址为： [https://developer.apple.com/contact/network-extension/](https://developer.apple.com/contact/network-extension/) 。

# 二. 创建 App ID

打开苹果开发者中心，登陆然后找到 App IDs 选项，点击右上角按钮创建一个 App ID 用于接下来创建 Provisioning Profile，地址为： [https://developer.apple.com/account/ios/identifier/bundle/](https://developer.apple.com/account/ios/identifier/bundle/)  ，如图所示：

![创建 App ID](/images/2017/NEHotspotHelper/1.jpg)

首先，填写 Name 以及 Bundle ID，这里统一填写为 EFNEHotspotHelperDemo，如图所示：

![填写 Name](/images/2017/NEHotspotHelper/2.jpg)

![填写 Bundle ID](/images/2017/NEHotspotHelper/3.jpg)

接下来这一步注意需要勾选 Wireless Accessory Configuration 这一选项，如图所示：

![勾选 Wireless Accessory Configuration](/images/2017/NEHotspotHelper/4.jpg)

然后观察到如图所示状态表明已成功打开：

![状态显示](/images/2017/NEHotspotHelper/5.jpg)

在 App IDs 列表中查看刚创建完成的 App ID：

![App IDs 列表](/images/2017/NEHotspotHelper/6.jpg)

# 三. 创建 Provisioning Profile

找到 Provisioning Profiles 选项，点击右上角按钮创建一个 Provisioning Profile 用于接下来创建示例工程，地址为： [https://developer.apple.com/account/ios/profile/](https://developer.apple.com/account/ios/profile/)  ，如图所示：

![创建 Provisioning Profile](/images/2017/NEHotspotHelper/7.jpg)

首先选择 Profile 类型，这里我选择的是 iOS App Development，可以根据自己的具体需要自由选择：

![选择 Profile 类型](/images/2017/NEHotspotHelper/8.jpg)

接下来选择我们在第二步创建好的 App ID，如图所示：

![选择 App ID](/images/2017/NEHotspotHelper/9.jpg)

然后选择证书和设备，全选即可：

![选择证书](/images/2017/NEHotspotHelper/10.jpg)

![选择设备](/images/2017/NEHotspotHelper/11.jpg)

在额外权限这一步需要选中我们申请到的 Network Extension 权限，可以看到其中包含我们需要使用的 NEHotspotHelper 权限，如图所示：

![选中 Network Extension 权限](/images/2017/NEHotspotHelper/12.jpg)

填写完 Profile Name 之后，即可成功创建我们需要的 Profile：

![填写 Profile Name](/images/2017/NEHotspotHelper/13.jpg)

点击 Download 将它下载到本地：
 
![下载 Profile](/images/2017/NEHotspotHelper/14.jpg)

双击打开，即可将 Profile 添加到本机：

![添加 Profile](/images/2017/NEHotspotHelper/15.jpg)

可以到 XCode 的账户设置里查看已安装的 Profile，若未安装成功可以尝试点击 Action 中的 Download 按钮重新下载：

![查看已安装的 Profile](/images/2017/NEHotspotHelper/16.jpg)

# 四. 创建工程

接下来我们创建一个示例工程，演示如何获取 WiFi 列表。首先，将 Bundle ID 改为之前设置的 EFNEHotspotHelperDemo：

![修改 Bundle ID](/images/2017/NEHotspotHelper/17.jpg)

然后在 Info.plist 中添加后台模式权限数组：

![添加后台模式代码](/images/2017/NEHotspotHelper/18.jpg)

代码如下：

```html
<key>UIBackgroundModes</key>
<array>
	<string>network-authentication</string>
</array>
```

添加完成后可以在 Target -> Capabilities 中看到后台模式已处于开启状态：

![后台模式已开启](/images/2017/NEHotspotHelper/19.jpg)

接下来在 Capabilities 找到 Wireless Accessory Configuration 并将其打开：

![打开 Wireless Accessory Configuration](/images/2017/NEHotspotHelper/20.jpg)

 在工程中找到后缀为 {工程名}.entitlements 的文件 EFNEHotspotHelperDemo.entitlements，在其中加入 HotspotHelper 权限代码：

![添加 HotspotHelper 权限代码](/images/2017/NEHotspotHelper/21.jpg)

代码如下：

```xml
<key>com.apple.developer.networking.HotspotHelper</key>
<true/>
```

好了，到这里已经完成了各种乱七八糟的配置工作，可以尝试进行 Build。如果没有提示错误信息的话，接下来就可以愉快地使用 HotspotHelper 了；如果有问题的话，请检查之前的步骤是否都已正确完成或者根据错误信息修改具体项目。

# 五. 核心代码

首先，在需要使用 HotspotHelper 的地方添加头文件引用，这里以 Objective-C 代码为例：

```
#import <NetworkExtension/NetworkExtension.h>
```

然后使用如下代码即可将 WiFi 列表信息打印到 XCode 控制台，注意：这里需要打开系统 `设置` 中的 `无线局域网` 页面才可以触发回调：

```
- (void)scanWifiInfos{
    NSLog(@"1.Start");

    NSMutableDictionary* options = [[NSMutableDictionary alloc] init];
    [options setObject:@"EFNEHotspotHelperDemo" forKey: kNEHotspotHelperOptionDisplayName];
    dispatch_queue_t queue = dispatch_queue_create("EFNEHotspotHelperDemo", NULL);

    NSLog(@"2.Try");
    BOOL returnType = [NEHotspotHelper registerWithOptions: options queue: queue handler: ^(NEHotspotHelperCommand * cmd) {

        NSLog(@"4.Finish");
        NEHotspotNetwork* network;
        if (cmd.commandType == kNEHotspotHelperCommandTypeEvaluate || cmd.commandType == kNEHotspotHelperCommandTypeFilterScanList) {
            // 遍历 WiFi 列表，打印基本信息
            for (network in cmd.networkList) {
                NSString* wifiInfoString = [[NSString alloc] initWithFormat: @"---------------------------\nSSID: %@\nMac地址: %@\n信号强度: %f\nCommandType:%ld\n---------------------------\n\n", network.SSID, network.BSSID, network.signalStrength, (long)cmd.commandType];
                NSLog(@"%@", wifiInfoString);

                // 检测到指定 WiFi 可设定密码直接连接
                if ([network.SSID isEqualToString: @"测试 WiFi"]) {
                    [network setConfidence: kNEHotspotHelperConfidenceHigh];
                    [network setPassword: @"123456789"];
                    NEHotspotHelperResponse *response = [cmd createResponse: kNEHotspotHelperResultSuccess];
                    NSLog(@"Response CMD: %@", response);
                    [response setNetworkList: @[network]];
                    [response setNetwork: network];
                    [response deliver];
                }
            }
        }
    }];

    // 注册成功 returnType 会返回一个 Yes 值，否则 No
    NSLog(@"3.Result: %@", returnType == YES ? @"Yes" : @"No");
}
```

# 六. 演示

唔，Demo 运行效果如下，点击 `Open WiFi Setting` 按钮可直接打开 `无线局域网` 页面：

![运行效果](/images/2017/NEHotspotHelper/22.png)

具体可尝试下载 Demo 并完成相应配置后体验：[https://github.com/EyreFree/EFNEHotspotHelperDemo](https://github.com/EyreFree/EFNEHotspotHelperDemo)

# 七. 备注

参考以下资料完成本 Demo，在此表示感谢：

[IOS NetworkExtension 框架使用笔记](http://blog.csdn.net/i374711088/article/details/51655526)
[iOS NEHotspotHelper使用](http://blog.csdn.net/toto18369905359/article/details/52622115)
[iOS-NetworkExtension-NEHotspotHelper](https://github.com/42vio/iOS-NetworkExtension-NEHotspotHelper)
[API Reference - NetworkExtension](https://developer.apple.com/reference/networkextension)

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://www.eyrefree.org/2017/NEHotspotHelper   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
