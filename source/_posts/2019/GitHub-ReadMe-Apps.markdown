---
layout: post
title: GitHub 项目 README 展示使用本开源库的 App
date: 2019-04-06 10:00:00 -05:00
categories: Code
tag: GitHub
---

## 零、前言

1. 本文展示有引用关系的 App，所以仅针对 iOS / Android 库；
2. 本文依赖 AppSight 的数据实现所述功能，若您的 SDK 无法在 AppSight 检索到，则本文方式无法使用。

## 一、找到源数据

展示使用某库的 App，首先需要找到一个能够提供相应数据的数据源，这里我们依赖的 [AppSight](https://www.appsight.io)，这里以 [EFCountingLabel](https://github.com/EFPrefix/EFCountingLabel) 为例，我们在 上找到它对应的页面 [https://www.appsight.io/sdk/ef-counting-label](https://www.appsight.io/sdk/ef-counting-label)，打开后可看到相关引用数据，如下所示：

![](/images/2019/GitHub-ReadMe-Apps/1.awebp)

## 二、加载所有数据

数据有了，接下来我们可以用脚本爬取的方式获取想要的数据，这里为了便于调试，笔者使用了 JavaScript，在 Chrome 的 Console 里执行就可以直接获取我们想要的结果。

由于此处的引用数据是有分页的，那么爬取之前，我们先要写一段脚本来循环点击分页加载按钮，帮助我们自动加载所有的分页数据（如果数据量少的话意义不是很大，但是数据量多的话，这一步就很有必要了）。

在 SDK 页面，打开 Chrome 开发者工具中的 `JavaScript 控制台`，执行如下脚本即可：

```javascript
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
async function main() {
	var using = document.getElementsByClassName('as-sec-apps-using')[0];
	do {
		if (document.getElementsByClassName('as-sec-loading')[0].style.cssText == "display: block;") {
			await sleep(1000);
		} else {
			if (using.getElementsByClassName('as-but as-but-more btn disabled')[0] == null) {
				using.getElementsByClassName('as-but as-but-more btn')[0].click();
				await sleep(1000);
			} else {
				break;
			}
		}
	} while (true);
}
main();
```

## 三、生成 HTML 代码

所有分页数据加载完成（或者达到你需要的数量）后，继续在 `JavaScript 控制台`，执行如下脚本：

```javascript
var using = document.getElementsByClassName('as-sec-apps-using')[0]
var items = using.getElementsByClassName('as-list-item-inner')
var result = "<table><tr>"
for(let i = 0, len = items.length; i < len; i++) {
	if(i % 10 == 0 && i > 0) {
		result = result + "</tr><tr>"
	}
    let item = items[i]
    var icon = item.getElementsByClassName('as-icon')[0].src;
    var title = item.getElementsByClassName('as-label as-name')[0].innerHTML;
    var href = item.href
    result = result + "<td><a href=\'" + href + "\' title=\'" + title + "\'><img src=\'" + icon + "\'></a></td>"
}
result = result + "</tr></table>"
console.log(result)
```

可以获得对应的 App 信息的 HTML 代码，如下所示：

![](/images/2019/GitHub-ReadMe-Apps/2.awebp)

获得的代码整理后为如下形式：

```html
<table>
  <tr>
    <td>
      <a href='https://www.appsight.io/app/toss-%ED%86%A0%EC%8A%A4' title='토스'>
        <img src='https://d3ixtyf8ei2pcx.cloudfront.net/icons/001/263/485/media/small.png?1530945069'>
      </a>
    </td>
    <td>
      <a href='https://www.appsight.io/app/%EC%87%BC%ED%95%91%EC%9D%84-%EB%9A%9D%EB%94%B1-%ED%8B%B0%EB%AA%AC' title='티몬 - 오늘은 또 어떤 딜?'>
        <img src='https://d3ixtyf8ei2pcx.cloudfront.net/icons/001/286/380/media/small.png?1534301992'>
      </a>
    </td>
    <td>
      <a href='https://www.appsight.io/app/%EB%B1%85%ED%81%AC%EC%83%90%EB%9F%AC%EB%93%9C' title='뱅크샐러드'>
        <img src='https://d3ixtyf8ei2pcx.cloudfront.net/icons/001/282/332/media/small.png?1533591669'>
      </a>
    </td>
    <td>
      <a href='https://www.appsight.io/app/climendo-basic' title='Climendo Basic'>
        <img src='https://d3ixtyf8ei2pcx.cloudfront.net/icons/000/304/533/media/small.png?1481531280'>
      </a>
    </td>
  </tr>
</table>
```

## 四、嵌入 README 中

复制我们前面获得的 HTML 代码，直接插入到我们项目 README 的合适位置即可：

![](/images/2019/GitHub-ReadMe-Apps/3.awebp)

## 五、效果展示

最后实际效果如下，也可以点击直接查看 [EFCountingLabel 的 README](https://github.com/EFPrefix/EFCountingLabel/blob/master/README.md) 进行预览：

![](/images/2019/GitHub-ReadMe-Apps/4.awebp)

感兴趣的同学，快自己动手试一试吧，👻

---

再读一篇类似文章？

[GitHub Wiki 页面的添加和设置](https://www.eyrefree.org/2017/07/06/GitHub-Wiki-Introduction/)

---

> 如有任何知识产权、版权问题或理论错误，还请指正。   
> https://www.eyrefree.org/2019/GitHub-ReadMe-Apps/   
> 如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
