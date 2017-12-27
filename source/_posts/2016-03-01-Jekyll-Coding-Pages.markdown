---
layout: post
title: OS X 下使用 Jekyll 和 Coding Pages 搭建静态博客
date: 2016-03-01 10:00:00 -05:00
categories: Blog
tag: Jekyll
---

Jekyll 是一个免费的简单静态网页生成工具，可以配合第三方服务例如 Disqus 实现一些扩展功能，不需要数据库支持。并且 Jekyll 可以部署在Github 或 Coding 上，可以绑定自己的域名，而且目前这是完全免费的。

---
# 1，Git 安装  
搭建博客需要用到 git，git --version 命令可查看本机是否已安装 git，若未安装可参考[这篇博文](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000/)进行安装。

# 2，Gem 安装/设置  
安装 Jekyll 需要用到包管理器 gem，gem -v 命令可查看本机是否已安装 gem，若未安装请自行安装。 
由于众所周知的原因，国内访问官方默认 gem sources 源速度不是十分理想，所以建议切换成国内的，利用 gem sources -l 命令可查看当前 gem sources 源：
``` bash
gem sources --remove http://rubygems.org/
```
然后利用以下命令将其替换为淘宝的（注意：这里的 http://rubygems.org/ 替换成当前 gem sources 源地址）： 
``` bash
gem sources -a https://ruby.taobao.org/
```

# 3，安装 Jekyll 到本地  
因为打算在 Coding Pages 上搭建，根据 [Coding 帮助文档](https://coding.net/help/doc/pages/index.html)，Coding Pages 目前支持 jekyll 2.4.0，所以我们需要指定版本安装 Jekyll，终端执行以下命令： 
``` bash
sudo gem install jekyll -v '2.4.0'
```
输入密码后等待安装完成，执行以下命令尝试查看 Jekyll 版本号： 
``` bash
jekyll -v
```
若能正确显示版本号 jekyll 2.4.0 表示安装成功。 

# 4，本地建立博客  
从零开始手动编写的话可以参考：[这篇博文](http://www.blogways.net/blog/2013/04/13/jekyll-usage.html)，同时网上有大量开发者们分享的模板可供选择使用：  
[Jekyll Themes](http://jekyllthemes.org/)  
[Github Jekyll Sites](https://github.com/jekyll/jekyll/wiki/Sites)  
本博客的搭建我选择了在该[模板](https://github.com/sl4m/skim.cc)的基础上进行修改，在这里对原作者表示感谢，🙏
在终端中切换到合适的目录下执行以下命令：
``` bash
git clone https://github.com/sl4m/skim.cc.git
```
将模板 git 仓库下载到本地。

# 5，本地效果预览  
终端中用 cd 命令切换到本地博客所在目录，即 skim.cc 目录下，执行 jekyll server 命令启动本地服务器，若启动成功可在浏览器中访问 http://0.0.0.0:4000/ 进行预览。 

# 6，上传到 Coding Pages  
在 Coding 新建一个项目，将博客所在项目 push 到新建的项目中，推荐的做法是创建一个新的 coding-pages 分支来作为启动 Coding Pages 之用（其他分支名也可以），然后切换到 Pages 标签，开启 pages 服务，分支名填写为我们需要的分支，这里是 coding-pages。

# 7，服务器效果预览  
这时 Coding 会提供一个类似 {user_name}.coding.me/{project_name} 的链接用于访问，例如本博客的是： 
``` bash
[http://eyrefree.coding.me/blog.eyrefree.org](http://eyrefree.coding.me/blog.eyrefree.org)
```

# 8，绑定域名  
默认提供的链接可能过长或者不便于日常使用，我们也可以绑定自己的域名。 
首先，需要提前准备一个域名，然后打开自己的域名控制面板，新建一个 CNAME 解析到 {user_name}.coding.me，例如我的是将 blog.eyrefree.org 解析到 eyrefree.coding.me； 
然后，打开 Coding 项目页面切换到 pages 项，填入刚才的设置解析的域名 blog.eyrefree.org，点击“添加域名绑定”按钮即可，在浏览器中直接访问 blog.eyrefree.org 就能成功打开。
有时可能由于缓存或者解析时间的问题，稍等片刻即可。 

# 9，编写博文  
接下来就是日常的博文编写啦，这里是使用 markdown 格式的，编写完成后添加到 _posts 目录下 push 到 Coding 服务器即可，具体可参考[这篇博文](https://segmentfault.com/a/1190000000406013)。

---
嘛，大概就是这些内容了，有遗漏的话后期会继续补充，😝，[我的博客](http://www.eyrefree.org)在原模版基础上将 Google 统计，Disqus 评论，feedburner 等替换为了自己的，其他的一些修改详情参见我的 Coding 仓库的 Jekyll 分支：
[https://coding.net/u/eyrefree/p/blog.eyrefree.org/git](https://coding.net/u/eyrefree/p/blog.eyrefree.org/git)

---
本文链接：[http://www.eyrefree.org/2016/03/01/2016-03-01-Jekyll-Coding-Pages/](http://www.eyrefree.org/2016/03/01/2016-03-01-Jekyll-Coding-Pages/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)