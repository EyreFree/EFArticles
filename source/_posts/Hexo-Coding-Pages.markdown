---
layout: post
title: OS X 下使用 Hexo 和 Coding Pages 搭建静态博客
date: 2016-03-23 10:00:00 -05:00
categories: Blog
tag: Hexo
---

[Hexo](https://github.com/hexojs/hexo) 是一款基于 Node.js 的静态博客框架, 目前在 GitHub 上已有 9133 star 和 1499 fork。Hexo 生成的静态页面可以部署在 Github 或 Coding 上，并且能够免费绑定自己的域名，可以用来很方便地搭建个人博客。

---
# 1，Git 安装  
搭建博客需要用到 git，下面这条命令可查看本机是否已安装 git，若未安装可参考[这篇博文](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000/)进行安装。
```bash
git --version
```

# 2，安装 Node.js
Mac下最简单的做法便是直接下载pkg文件进行安装，最新版本的下载地址如下，选择后缀为pkg的文件下载安装即可：  
[https://nodejs.org/download/release/latest/](https://nodejs.org/download/release/latest/)  
完装完成后，要将以下路径计入你的系统环境变量 /usr/local/bin，步骤如下：
用vim 打开该文件：
```bash
cd; vi .bash_profile
```
在文件中加入该语句：
```bash
export PATH=/usr/local/bin:$PATH
```
并保存退出，重新加载shell让设置的环境变量生效：
```bash
source ~/.bash_profile
```

# 3，将 npm 的源替换成淘宝的源
由于众所周知的原因，国内访问官方默认 npmjs.org 源速度不是十分理想，所以建议切换成国内的，利用以下命令将其替换为淘宝的 npm 源： 
```bash
npm config set registry http://registry.npm.taobao.org/
```

# 4，安装 Hexo
利用 npm 命令安装：
```bash
sudo npm install -g hexo
```
因为可能有文件读写权限的问题，这里推荐用 sudo，输入密码后会开始安装，时间可能比较长，耐心等待，如果长时间卡在某一步 Ctrl + C 终止当前任务后重试即可。

# 5，本地建立博客  
安装完成后，新建一个目录如 myblog 用于存放博客，切换到该目录下执行以下指令，Hexo 即会在目标文件夹初步生成博客所需要的所有文件：
```bash
hexo init
```
然后切换到该目录下执行如下命令，安装所需要的依赖：
```bash
sudo npm install
```
网上有大量开发者们分享的模板可供选择使用，将它们的 Git 仓库 Clone 以后放到博客目录下的 themes 文件夹中即可：  
[Github Hexo Themes](https://github.com/hexojs/hexo/wiki/Themes)  
[有哪些好看的 Hexo 主题？](http://www.zhihu.com/question/24422335)  
本博客的搭建我选择了使用该主题：  
https://github.com/forsigner/fexo  
在这里对原作者 forsigner 表示感谢，🙏

# 6，安装 Server 组件
保持在本地博客路径下，在终端中执行如下命令：
```bash
hexo
```
因为这并不是一个完整的命令，所以这时我们可以看到 hexo 输出的帮助信息，如下图所示：

<center>
![Hexo-Coding-Pages-1](/images/Hexo-Coding-Pages-1.png)
</center>

我们可以在左边的 Commands 列表中查看我们需要的命令是否已成功安装，因为某些版本的 Hexo 的 Server 模块需要独立安装所以这里我们并没有看到 server 命令，我们可以执行下面这条命令来进行安装，如果有的话可以跳过这一步：
```bash
sudo npm install hexo-server --save
```
如果安装过程中出现一些问题导致某些模块没有安装成功的也可以通过类似的方式单独安装某个模块进行修复。

# 7，安装 RSS 插件（可忽略）  
到博客所在目录 myblog 下，利用该命令安装 RSS 插件，暂时不需添加 RSS 功能的同学可忽略该步骤：
```bash
sudo npm install hexo-generator-feed --save
```
编辑 myblog 目录下的 _config.yml，添加如下代码开启 RSS 功能：  
```bash
rss: /atom.xml
```
RSS 地址保持默认即可，不需要多做修改。

# 8，本地效果预览  
在终端使用 cd 命令切换到博客所在目录 myblog，执行如下命令生成静态博客页面并启动本地服务器，若成功可在浏览器中访问 http://localhost:4000/ 进行预览。 
```bash
hexo generate
hexo server
```
或者如下的缩写形式也可以：
```bash
hexo g
hexo s
```

# 9，部署到 Coding Pages  
在 Coding 新建一个项目，假设为 myblog，然后修改本地博客目录下的 _config.yml 文件，根据[官方文档](https://hexo.io/docs/deployment.html)的描述，修改以下几个参数，这些参数一般在文件底部：
```bash
deploy:
type: git               #部署方式，这里我们用的是Coding的Git
repo: <repository url>  #仓库地址，例如我的是git@git.coding.net:eyrefree/myblog.git   
branch: [branch]        #分支名，可任意填写，我填写的是master
message: [message]      #可不填，这是显示在提交记录里的描述信息，默认为日期
```
参数修改完成后，我们需要在终端中切换到博客所在目录安装 deploy 组建，执行以下命令将生成的博客静态页面 push 到我们上面在 Coding 创建的 myblog 仓库中：
```bash
sudo npm install hexo-deployer-git --save
```
然后执行依次执行清理命令：
```bash
hexo clean
```
生成命令：
```bash
hexo g
```
部署命令：
```bash
hexo d
```
如果在 _config.yml 的 repo 处填写的仓库地址是 https 形式的，在部署时可能需要输入你的 Coding 账号和密码。  
然后切换到该项目的 Pages 标签，开启 pages 服务，分支名填写为我们在_config.yml 文件中设定的分支，我的是 master。

# 10，服务器效果预览  
pages 服务开启完成后，Coding 会提供一个类似 {user_name}.coding.me/{project_name} 的链接用于访问，例如用户名为 eyrefree 项目名为 myblog 的链接应该是： 
```bash
http://eyrefree.coding.me/myblog
```
访问该链接即可进行预览，由于我们引用的资源很多是和域名相关的，导致这里可能会有资源加载失败的情况，只能出现部分文字，接下来我们将域名绑定后即可恢复正常。

# 11，绑定域名  
默认提供的链接可能过长或者不便于日常使用，我们也可以绑定自己的域名。  
首先，需要提前准备一个域名，然后打开自己的域名控制面板，新建一个 CNAME 解析到 {user_name}.coding.me，例如我的是将 www.eyrefree.org 解析到 eyrefree.coding.me；  
然后，打开 Coding 项目页面切换到 pages 项，填入刚才的设置解析的域名 www.eyrefree.org，点击“添加域名绑定”按钮即可，在浏览器中直接访问 www.eyrefree.org 就能成功打开。  
有时可能由于缓存或者解析时间的问题，稍等片刻即可。 

# 12，编写博文  
接下来就是日常的博文编写啦，这里是使用 markdown 格式的，编写完成后添加到 source/_posts 目录下然后按照第 8 步的方法部署到 Coding 服务器即可，具体可参考[这篇博文](http://www.jianshu.com/p/3c7ddd48bfa9)，Markdown 的一些语法可以参考：  
[http://wowubuntu.com/markdown/](http://wowubuntu.com/markdown/)

# 13，常见问题
若执行 hexo 命令时出现如下警告信息：
```bash
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```
可以尝试执行以下命令修复：
```bash
sudo npm install hexo --no-optional
```

---
嘛，大概就是这些内容了，有遗漏的话会继续补充，😝。[我的博客](http://www.eyrefree.org)是用 Hexo 生成的使用了 Fexo 模版，开启了 Google 统计，Disqus 评论，RSS 订阅，站内搜索等，详情参见我的 Coding 仓库的 Hexo 分支：
[https://coding.net/u/eyrefree/p/blog.eyrefree.org/git](https://coding.net/u/eyrefree/p/blog.eyrefree.org/git)

---
本文链接：[http://www.eyrefree.org/2016/03/23/2016-03-23-Hexo-Coding-Pages/](http://www.eyrefree.org/2016/03/23/2016-03-23-Hexo-Coding-Pages/)

如文中无特殊说明，本站均使用以下协议保护：[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/3.0/cn/)
