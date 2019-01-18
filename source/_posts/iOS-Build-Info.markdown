---
layout: post
title: iOS 在 App 中获取 XCode 构建信息
date: 2016-03-08 10:00:00 -05:00
categories: iOS
tag: Nothing
---

iOS 在 App 中获取当前版本的构建时间和 Git Hash 值，Demo 地址：  
[https://github.com/EyreFree/EFBuildInfoDemo](https://github.com/EyreFree/EFBuildInfoDemo)

---
# 1.添加 Run Script
打开需要获取构建信息的工程，选中工程，切换到 Build Phases 选项卡，点击左边的“+”号选择“New Run Script Phase”一项添加一个新的 Run Script：

<center>
![iOS-Build-Info-1](/images/iOS-Build-Info-1.png)
</center>

并将其命名为“Build Config"，然后将其拖动到“Target Dependencies”的下面：

<center>
![iOS-Build-Info-2](/images/iOS-Build-Info-2.png)
</center>

# 2.为 Run Script 添加代码
点开“Build Config"左边的小三角，在其中填写如下代码：
```bash
myFile="BuildConfig.plist"

myDate=`date +%Y-%m-%dT%H:%M:%S%z`
echo $myDate
myHash=`git rev-parse --short HEAD`
echo $myHash

if [ ! -f "$myFile" ]; then
    /usr/libexec/PlistBuddy -c "Add :BUILD_TIME string $myDate" "$myFile"
    /usr/libexec/PlistBuddy -c "Add :GIT_SHA string $myHash" "$myFile"
else
    /usr/libexec/PlistBuddy -c "Set :BUILD_TIME $myDate" "$myFile"
    /usr/libexec/PlistBuddy -c "Set :GIT_SHA $myHash" "$myFile"
fi
```

<center>
![iOS-Build-Info-3](/images/iOS-Build-Info-3.png)
</center>

# 3.生成 BuildConfig.plist 文件
然后我们编译一次就可以发现，这段代码会在编译时在工程所在目录下生成一个 BuildConfig.plist 文件，其中包含了本次构建的时间和当前版本的 GitHash，以后每一次 XCode 构建时都会自动更新该文件，详细内容如下：
```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>BUILD_TIME</key>
        <string>2016-03-30T09:38:04+0800</string>
        <key>GIT_SHA</key>
        <string>3e4d213</string>
</dict>
</plist>
```

<center>
![iOS-Build-Info-4](/images/iOS-Build-Info-4.png)
</center>

# 4.获取 BuildConfig.plist 文件内容
将 BuildConfig.plist 添加到我们的工程中：

<center>
![iOS-Build-Info-5](/images/iOS-Build-Info-5.png)
</center>

然后在需要获取构建信息的位置添加如下代码就能成功获取构建时间和 Git Hash 值：
```swift
//输出构建信息
var bulidTime: String!
var gitSha: String!
if let buildConfigFilePath = NSBundle.mainBundle()
    .pathForResource("BuildConfig", ofType: "plist") {
    if let dict = NSDictionary(contentsOfFile: buildConfigFilePath) {
        bulidTime = dict["BUILD_TIME"] as? String ?? "未知"
        gitSha = dict["GIT_SHA"] as? String ?? "未知"
    }
}
print("BUILD_TIME: \(bulidTime)")
print("GIT_SHA: \(gitSha)")
```
结果如图所示：

<center>
![iOS-Build-Info-6](/images/iOS-Build-Info-6.png)
</center>

---
本文链接：[http://www.eyrefree.org/2016/03/08/2016-03-08-iOS-Build-Info/](http://www.eyrefree.org/2016/03/08/2016-03-08-iOS-Build-Info/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
