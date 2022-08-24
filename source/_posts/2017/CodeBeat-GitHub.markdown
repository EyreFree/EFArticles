---
layout: post
title: 利用 CodeBeat 为你在 GitHub 上的项目进行代码质量管理
date: 2017-12-13 10:00:00 -05:00
categories: Code
tag: CodeBeat
---

CodeBeat 是一个免费为开源项目进行代码质量管理的工具（付费可以支持私有项目），目前已经支持的编程语言有 Swift、Objective-C、Go、Ruby、Python、Java、Kotlin、Javascript、Typescript、Elixir，无需对原有项目进行任何修改即可获取针对项目的完整质量分析，方便快捷。

## 前言

当我们在 GitHub 上的代码仓库发生变更后，会通知 CodeBeat 执行分析操作刷新项目代码质量评分，并在完成后刷新项目评级 / 评分的状态或结果，如图所示：

![代码质量效果预览](/images/2017/CodeBeat-GitHub/1.awebp)

CodeBeat 的同类产品有 Code Climate，目前支持 Ruby、Python、PHP、JavaScript、Java、TypeScript，不过官网显示
 Swift、Go、Objective-C 的支持在计划中，因为我是 iOS 开发，所以暂时用不了这个，在一个 [Ruby 项目](https://github.com/BigKeeper/bigstash)有试过这个，看起来还好，有兴趣的同学也可以一试。

本文以 [EFQRCode](https://github.com/EyreFree/EFQRCode)(一个使用 Swift 作为开发语言的二维码库) 为例，简述怎样为自己的开源项目添加代码质量管理功能。

## 1. 注册 CodeBeat 账号

打开 [https://codebeat.co/](https://codebeat.co/) 注册一个 CodeBeat 账号，也可以通过 GitHub 账户直接登陆。CodeBeat 服务对开源项目是免费的，所以你的私有项目无法享受到免费的持续构建服务。唔，当然，每月支付 20 美刀成为付费用户后可以解锁无限数量私有库的功能。

## 2. 从 GitHub 添加项目

登陆完成后，点击右边的 `Add Repository` 按钮即可开始添加自己的 Git 仓库，支持各种 Git 托管平台，甚至自建的也可以：

![Add Repository](/images/2017/CodeBeat-GitHub/2.awebp)

## 3. 开启代码质量管理

第一次项目导入后会立即进行一次分析，试了一下速度还是比较快的（反正比持续集成快多了），反正我的项目导入以后刷新一下页面就出结果了。

![](/images/2017/CodeBeat-GitHub/3.awebp)

唔，细心的同学可能会发现，这一步操作完成后我们在 GitHub 项目 Setting 中的 `Webhooks` 已经添加了一个属于 `codebeat.co` 的 Webhook，没错，以后项目代码发生更改后就会自动触发代码质量分析，不需要我们手动操作了。感兴趣的同学可以点击 `Edit` 按钮查看一下 CodeBeat 具体干了啥：

![Webhooks](/images/2017/CodeBeat-GitHub/4.awebp)

关于 Webhook 感兴趣的同学可以查看 GitHub 官方的资料：[https://developer.github.com/webhooks/](https://developer.github.com/webhooks/)。

## 4. 查看代码质量分析结果

点击进入该项目的分析结果，可以查看到具体的问题，如代码复杂性、代码风格、代码重复等，点击 `Quick Wins` 这一栏可以查看优先推荐修复的项目，如下图所示：

![](/images/2017/CodeBeat-GitHub/5.awebp)

我们可以对应分析出的代码质量问题对我们的工程代码进行修改，改完直接提交到仓库即可，Webhook 会通知 CodeBeat 进行刷新。

## 没了

在  的项目设置中可以看到更多有意思的玩法，比如将代码质量变化通知发送到 Slack 或邮箱等，也可以将代码质量评级徽标添加到自己的项目 README 中，大佬们可以自行研究...

![Setting](/images/2017/CodeBeat-GitHub/6.awebp)

祝操作顺利，🌈

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://eyrefree.org/2017/CodeBeat-GitHub   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
