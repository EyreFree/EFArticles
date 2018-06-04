---
layout: post
title: iOS Markdown 转换及预览
date: 2017-08-27 10:00:00 -05:00
categories: iOS
tag: Markdown
---

作为一个开发人员，日常经常会需要编写各种各样的文档／材料之类的，个人非常喜欢用 Markdown 来完成这些工作，Markdown 的优点就不再赘述了，大家应该都有过了解，不过目前 iOS 原生并没有提供任何对 Markdown 的支持。所以最近基于 cmark-gfm 把 Markdown 转 HTML 的功能封装了一遍，并且在原有基础上添加了对列表 table 的支持，同时利用 WKWebView 做了一个可直接展示 Markdown 的 View，方便以后使用，现已开源到 GitHub 基于 WTFPL 协议进行分发，需要的同学可以自取。

项目地址：[https://github.com/EyreFree/EFMarkdown](https://github.com/EyreFree/EFMarkdown)

---

![](http://upload-images.jianshu.io/upload_images/1018190-4e62a5fdcac2b5c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

EFMarkdown 是一个轻量级的 Markdown 库，可以用来将 Markdown 转为 HTML，也可以用来直接展示 Markdown 对其进行预览。

> [English Introduction](https://github.com/EyreFree/EFMarkdown/blob/master/README.md)

## 预览

sample1|sample2|sample3|sample4  
:---------------------:|:---------------------:|:---------------------:|:---------------------:
![](http://upload-images.jianshu.io/upload_images/1018190-da0a10c955e06239.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1018190-6812167f568aef5d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1018190-64e5e2ae106fb89d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|![](http://upload-images.jianshu.io/upload_images/1018190-90d2e5d82627276b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

## 示例

1. 利用 `git clone` 命令下载本仓库；
2. 利用 cd 命令切换到 Example 目录下，执行 `pod install` 命令；
3. 随后打开 `EFMarkdown.xcworkspace` 编译即可。

或执行以下命令：

```bash
git clone git@github.com:EyreFree/EFMarkdown.git; cd EFMarkdown/Example; pod install; open EFMarkdown.xcworkspace
```

## 环境

- XCode 8.0+
- Swift 3.0+

## 安装

EFMarkdown 可以通过 [CocoaPods](http://cocoapods.org) 进行获取。只需要在你的 Podfile 中添加如下代码就能实现引入：

```
pod "EFMarkdown"
```

## 使用

### 1. 将 Markdown 转为 HTML

你可以利用 `EFMarkdown` 轻松实现 Markdown 字符串到 HTML 字符串地转换，示例代码如下：

```swift
let markdown = "# Hello"
var html = ""
do {
    html = try EFMarkdown().markdownToHTML(markdown, options: EFMarkdownOptions.safe)
    print(html) // 这里会输出 "<h1>Hello</h1>\n"
} catch let error as NSError {
    print ("Error: \(error.domain)")
}
```

### 2. 对 Markdown 进行预览

你可以利用 `EFMarkdownView` 实现对 Markdown 字符串的预览，示例代码如下：

```swift
let screenSize = UIScreen.main.bounds
let markView = EFMarkdownView()
markView.frame = CGRect(x: 0, y: 20, width: screenSize.width, height: screenSize.height - 20)
self.view.addSubview(markView)
markView.load(markdown: testMarkdownFileContent(), options: [.default]) {
    [weak self] (_, _) in
    if let _ = self {
        // 可选：你可以通过在此处传入一个百分比来改变字体大小
        markView.setFontSize(percent: 128)
        printLog("load finish!")
    }
}
```

### 3. 选项

你可以通过传入不同的选项来控制底层 `cmark` 对 Markdown 字符串的处理，默认传入的值为 `safe`。

可选的值有以下这些：

* default
* sourcePos
* hardBreaks
* safe
* noBreaks
* validateUTF8
* smart
* githubPreLang
* liberalHtmlTag

更多关于这些选项的信息，可以参考 [`cmark`](https://github.com/github/cmark)。

## 作者

EyreFree, eyrefree@eyrefree.org

## 协议

![](http://upload-images.jianshu.io/upload_images/1018190-17002c4b490b0b86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

EFMarkdown 基于 WTFPL 协议进行分发和使用，更多信息参见协议文件。

---

本文链接：[http://www.eyrefree.org/2017/08/27/2017-08-27-EFMarkdown/](http://www.eyrefree.org/2017/08/27/2017-08-27-EFMarkdown/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
