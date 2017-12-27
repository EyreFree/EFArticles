---
layout: post
title: AppStore 审核 macOS 应用踩坑记录
date: 2017-12-12 10:00:00 -05:00
categories: AppStore
tag: macOS
---

## 1. Guideline 2.3.8 - Performance

```
Guideline 2.3.8 - Performance

We noticed that your app name to be displayed on the App Store does not sufficiently match the name of the app displayed when installed on macOS.

iTunes Connect Name: EFQRCode

App Name when Installed: EFQRCode.macos

App Name when Launched: EFQRCode

App Name in About/Quit Menu: macOS Example
```

大概是说 AppStore 的应用名称和 App 安装后以及 App 内的菜单项目上显示的不符，需要修改每一处到一样后重新打包提交。

iTunes Connect Name：编辑 iTunes Connect 的 App 信息中 App 名称可更改；
App Name when Installed：编辑 Build Setting 中的 Product Name 可更改；

## 2. Guideline 5.2.5 - Legal

```
Guideline 5.2.5 - Legal

Your app uses ‘macOS’ in the Installed App Name and Menu Item Names in a manner that is not consistent with Apple's trademark guidelines.

Indicating Mac compatibility in the app name is not necessary for the Mac App Store.
```

这条意思是 App 中不应该出现 ‘macOS’ 等不符合苹果商标指南的字样，全局搜索，把不合法的文字出现从按钮 / 菜单 / 页面去除即可。

---

本文链接：[http://www.eyrefree.org/2017/12/12/2017-12-12-AppStore-macOS/](http://www.eyrefree.org/2017/12/12/2017-12-12-AppStore-macOS/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
