---
layout:     post
title:      "生活就是不断解决问题"
subtitle:   "解决问题也是意味着付出和收获"
date:       2016-06-26
author:     "SixKarma"
header-img: "img/post-bg-idea.jpg"
catalog: true
tags:
    - 生活
    - IT
---

> Lots of things proved that I am a little perfectionis.<br>

## 前传
首先，GitHub上注册，新建一个repository，注意名称；<br>
然后，`git clone github.com/user/repository.git`到本地计算机；<br>
其次，删除所有文件，除了.git隐藏文件夹；<br>
再次，找到一份自己看起来比较Prefer的Theme，然后Fork或者Clone，修改_config.yml配置文件；<br>
最后，`git push origin master`到远程。<br>
本地环境：github+markdown+jekyll<br>
Git命令：<br>
暂存：git add .<br>
提交：git commit -m "blog changes"<br>
关联：git remote add origin git@github.com:sixkarma/sixkarma.github.io.git<br>
推送：git push -u origin master<br>
克隆：git clone git@github.com:sixkarma/sixkarma.github.io.git<br>
查看分支：git branch<br>
创建分支：git branch <name><br>
切换分支：git checkout <name><br>
创建+切换分支：git checkout -b <name><br>
合并某分支到当前分支：git merge <name><br>
删除分支：git branch -d <name><br>
$ bundle exec jekyll server<br>
$ jekyll serve<br>
$ jekyll serve --watch


## 问题描述
前前后后整理了2天，`bundle exec jekyll server`后，在本地浏览`http://127.0.0.1:4000`一切正常，但是`git push origin master`到github后，底部社交媒体的链接图标却无法正常显示，如下图所示。<br>
<img class="shadow" src="/img/in-post/post-blog-withno-contact.jpg" width="971">
<br>一开始以为是_config.yml有错误，反复检查了很多遍,与显示正常的网页逐条比较，昨晚一直调试到凌晨1点多，仍然找不出问题。<br>
换了不同浏览器，效果一样，按F12进入开发者模式，发现无法显示的原因在于未能正确加载`Font Awesome`字体库，问题基本可以确定了，但是根源还未找到，本地加载正常，而push到远程则不显示，怀疑浏览器端问题，或者是github需要设置某项参数。

## 推导验证
首先，验证浏览器端，将360浏览器切换为兼容模式，加载速度变慢，但是正常显示，关键在于给出安全提示“加载不安全脚本”，且URL输入框显示“证书风险”，从而发现我的blog网址为`https://sixkarma.github.io/`，而显示正常的Blog都是采用`http`协议。什么是`https`呢？`http`和`https`的区别，百度如下：<br>
> 1. https协议需要到ca申请证书，一般免费证书很少，需要交费。<br>
> 2. http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议。<br>
> 3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。<br>
> 4. http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

采用`https`的服务器必须从CA （Certificate Authority）申请一个用于证明服务器用途类型的证书。该证书只有用于对应的服务器，客户端才信任此主机。


问题范围进一步缩小，那么能不能把网址改成`http`呢？这就是第二个疑点：github参数设置。
<img class="shadow" src="/img/in-post/post-github-https.jpg" width="366">
<br>从上图来看还真是巧了，github从2016-06-15才强制要求必须采用`https`协议，这也是为什么同一套Theme，别人的都能正常显示，而我无论如何怎么改都不对。这个要求实际上目的是提高安全性，而带来的问题如何解决呢？显然，改成`http`是不可行的。

## 解决方法
经过上述的推导以及验证，已经明确定位到问题的根源，采用`https`协议导致`Font Awesome`字体库无法加载。
<br>这个字体库在`_includes/head.html`文件中，采用cdn外部链接的方式：
<br>`<link href="http://cdn.staticfile.org/font-awesome/4.2.0/css/font-awesome.min.css" rel="stylesheet" type="text/css">`
因此，链接方式`http`导致`https`无法加载该样式文件。解决方式也很简单了，改成如下：
<br>`<link href="https://cdn.staticfile.org/font-awesome/4.2.0/css/font-awesome.min.css" rel="stylesheet" type="text/css">`
<br>所谓，难者不会，会者不难吧。效果如下图：
<br>
<img class="shadow" src="/img/in-post/post-blog-with-contact.jpg" width="875">
<br>

## 后记
已经为第一篇博客写什么焦头烂额了，没想到好不容易Push上去之后，还有Bug，虽然社交媒体链接无可无不可，但看着总不爽，一心想着把问题解决，这好像也是我做事的思路和风格，也经常会反思，过于关注细节往往看不到全局，容易陷入芝麻绿豆这样的小事中；但做技术的，往往重要的就在细节问题上，没有一份坚持探索的精神，容易流于假大空。也许，的确是平衡。

