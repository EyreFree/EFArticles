---
layout: post
title: iOS 获取当前 WiFi 信息
date: 2016-03-30 10:00:00 -05:00
categories: iOS
tag: Nothing
---

此处以 Swift 代码为例

---
# 1.添加模块引用
首先我们在需要获取 WiFi 信息的地方引用需要的模块：
```swift
import SystemConfiguration.CaptiveNetwork
```

# 2.添加获取代码
接下来编写获取 WiFi 信息的代码，如下：
```swift
//获取 WiFi 信息
func getWifiInfo() -> (ssid: String, mac: String) {
    if let cfas: NSArray = CNCopySupportedInterfaces() {
        for cfa in cfas {
            if let dict = CFBridgingRetain(
                CNCopyCurrentNetworkInfo(cfa as! CFString)
                ) {
                if let ssid = dict["SSID"] as? String,
                    let bssid = dict["BSSID"] as? String {
                    return (ssid, bssid)
                }
            }
        }
    }
    return ("未知", "未知")
}
```

# 3.获取 WiFi 信息
然后在我们需要获取 WiFi 信息的位置添加如下代码即可：
```swift
let wifiInfo = getWifiInfo()
NSLog("SSID(WiFi名称): \(wifiInfo.0)")
NSLog("BSSID(Mac地址): \(wifiInfo.1)")
```

# 4.输出结果

<center>
![iOS-WiFi-Info-1](/images/iOS-WiFi-Info-1.png)
</center>

---
本文链接：[http://www.eyrefree.org/2016/03/30/2016-03-30-iOS-WiFi-Info/](http://www.eyrefree.org/2016/03/30/2016-03-30-iOS-WiFi-Info/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)

