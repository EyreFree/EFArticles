---
layout: post
title: 怎样将信息发布 / 记录到 ETH 网络？
date: 2018-07-25 10:00:00 -05:00
categories: BlockChain
tag: ETH
---

现在各行各业都在“上链”，大家说的“上链”到底是什么呢？今天，咱就来实际操作一把...

---

阅读本文前，请先确保您：

1. 具备科学上网能力；   
2. 有钱，能兑换成 ETH 虚拟币用来支付以太坊转账费手续费；   
3. 有一定的英文阅读能力。

---

## 1. 下载 / 安装 Chrome

下载谷歌浏览器并安装，官网地址：[https://www.google.com/intl/zh-CN_ALL/chrome/](https://www.google.com/intl/zh-CN_ALL/chrome/)

![](https://upload-images.jianshu.io/upload_images/1018190-ebc9cd894d955ba7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2. 安装 MetaMask 插件

打开 Chrome 网上应用店，下载 MetaMask 插件 [https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=zh-CN](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=zh-CN)，这是一个运行在 ETH 网络上的 DAPP，感兴趣的同学可以研究一下它的源码：[https://github.com/MetaMask/metamask-extension](https://github.com/MetaMask/metamask-extension)。

![](https://upload-images.jianshu.io/upload_images/1018190-31d82c6a4c6dd257.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后在 Chrome 内打开 MetaMask，左上角下拉列表可以选择网络类型，我们要用的是第一个 Main Ethereum Network（其他的都是测试网络...）：

![](https://upload-images.jianshu.io/upload_images/1018190-5a971e2759060a55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3. 注册 ETH 钱包

接下来在 MetaMask 内根据提示注册 ETH 钱包（同时也会成为你的 MetaMask 账号），注意将公钥、私钥、助记词、密码之类的信息记录在可靠的地方，丢失的话，你的 ETH 钱包（主要是里面包含的虚拟币）就没啦。

![](https://upload-images.jianshu.io/upload_images/1018190-0466beb4abcea283.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4. 给 ETH 账号打钱

新建的 ETH 钱包是木有钱的，而接下来的我们发布信息的操作需要执行转账动作。有的同学可能会问，我们进行一笔总价为 0 ETH 的交易不就行了么？没错，这样的确可以不产生实际的虚拟币转账，不过仍然需要手续费来驱使矿工们将这一笔为 0 的交易记录写入到区块内，也就是每一笔交易只有支付了手续费才有可能发生（手续费是交易发起者自定的，如果手续费过低，可能会出现交易失败）。

ETH 网络上用到的手续费肯定就是 ETH（以太坊）啦，来源的话，一般是去 [币安](https://www.binance.com/?ref=27965336)、[OTCBTC](https://otcbtc.com/referrals/EYREFREE) 之类的交易所购买然后从交易所提币到自己的钱包，过程比较繁琐，可以自行研究，这里不多做赘述。

只是了解 / 试用一下，不打算大批量购买的话，找一个有 ETH 的朋友让他转你 0.01 ETH（现价大概 3208.36 * 0.01 = 32 RMB）一般就够用了...

![](https://upload-images.jianshu.io/upload_images/1018190-e4207d8a72600521.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 5. 准备需要发布的信息

接下来就是准备我们需要发布的信息啦，因为需要转码成 Hex String，所以直接试用中文大概是不资瓷的啦，需要先转成拼音，比如我们随便找一段文字，像这样：

```
烂是有原因的，[微笑]。大家都愿意吃屎，你不吃，就是你的过错了，[微笑]。***写得再烂，至少还愿意认错，[微笑]。****不止烂，还嘴硬，[微笑]
```

然后用 [汉字转拼音工具](https://www.chineseconverter.com/zh-cn/convert/chinese-to-pinyin) 转为拼音（注意全角标点符号要自己改成半角哦，然后风格大家可以自选...）：

```
lan4 shi4 you3 yuan2 yin1 di2 , [ wei1 xiao4 ] . da4 jia1 du1 yuan4 yi4 chi1 shi3 , ni3 bu4 chi1 , jiu4 shi4 ni3 di2 guo4 cuo4 liao3 , [ wei1 xiao4 ] . * * * xie3 de2 zai4 lan4 , zhi4 shao3 huan2 yuan4 yi4 ren4 cuo4 , [ wei1 xiao4 ] . * * * * bu4 zhi3 lan4 , huan2 zui3 ying4 , [ wei1 xiao4 ] .
```

![](https://upload-images.jianshu.io/upload_images/1018190-a84204c59e10d4dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来再用 [String 转 Hex 工具](https://codebeautify.org/string-hex-converter) 转为 Hex String 即可：

```
6c616e34207368693420796f7533207975616e322079696e3120646932202c205b2077656931207869616f34205d202e20646134206a69613120647531207975616e342079693420636869312073686933202c206e6933206275342063686931202c206a6975342073686934206e6933206469322067756f342063756f34206c69616f33202c205b2077656931207869616f34205d202e202a202a202a207869653320646532207a616934206c616e34202c207a686934207368616f33206875616e32207975616e34207969342072656e342063756f34202c205b2077656931207869616f34205d202e202a202a202a202a20627534207a686933206c616e34202c206875616e32207a7569332079696e6734202c205b2077656931207869616f34205d202e
```

![](https://upload-images.jianshu.io/upload_images/1018190-96ba6002d9f3ab2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后可以用 [Hex 转 String 工具](https://codebeautify.org/hex-string-converter) 试试看能不能再转回来确认一下有无问题：

![](https://upload-images.jianshu.io/upload_images/1018190-96390e6e4d7c14a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 6. 生成一笔交易记录

然后我们继续回到 MetaMask，点击 SEND 按钮开始一笔转账：

![](https://upload-images.jianshu.io/upload_images/1018190-ae93d36b52890b79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

随便填入一个有效的 ETH 接收地址，这里我用的是 0xa666b081583dbe8018af7c7c6e8bb6954c8984d2，然后交易数额填 0，TRANSACTION DATA 就填写刚才生成的 Hex String，然后点击 SEND 就行啦：

![](https://upload-images.jianshu.io/upload_images/1018190-806b97ab28a7070b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来是交易确认界面，我们需要将 Gas Price 修改为 2，这样容易更快完成交易：

![](https://upload-images.jianshu.io/upload_images/1018190-d094cb44fa8ebdb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后点击 SUBMIT 按钮即可，然后过一会就能在列表中看到这一条交易已完成啦：

![](https://upload-images.jianshu.io/upload_images/1018190-4c47251c48893fd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击可以查看详情，比如这一条交易记录可以在这个页面进行查看：[https://etherscan.io/tx/0xff61b121aef8065e6e0a2aeeff5d9fce738b1ba0cbe5e1cd8b51b3509faff903](https://etherscan.io/tx/0xff61b121aef8065e6e0a2aeeff5d9fce738b1ba0cbe5e1cd8b51b3509faff903)，翻到下面详情中的 Input Data 选择以 UTF-8 方式预览即可：

![](https://upload-images.jianshu.io/upload_images/1018190-409c6b0a8383e8a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后全球所有的 ETH 钱包这时应该都已经同步了这条信息啦，好玩吧，这，就是传说中的，上链。

还不赶紧自己动手实践一下？​



---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://www.eyrefree.org/2018/07/25/ETHTransactionData   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)。   
