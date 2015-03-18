---
title : Octopress改头换面
categories : 学习思考
tags : 
	- Octopress
---

用上了Octopress，自然要换换样式，默认的主题不错，就在这基础上稍微改改吧。在这其中遇到不少问题，挑几个来说一说。

###Sass
更改样式时最好不要直接改CSS，因为Octopress提供了强大的[Sass](http://sass-lang.com/)支持，所以，去`sass/custom`中去修改吧。关于Sass和Less谁更强大的问题，网上也是众说纷纭。官网首页第一句话很有意思：Sass让CSS焕发第二春（“Sass makes CSS fun again”）。

这里贴几篇文章供参考：

> - [Css预处理器实践之Sass、Less大比拼](http://cued.xunlei.com/log044)
- [LESS介绍及其与Sass的差异](http://www.qianduan.net/an-introduction-to-less-and-comparison-to-sass.html)
- [为您详细比较三个 CSS 预处理器（框架）：Sass、LESS 和 Stylus](http://www.oschina.net/question/12_44255)

<!--more-->

###JQuery冲突
本想用JQuery实现back-to-top,没想到把JQuery载入以后sidebar没法collapse了，google了一下发现Octopress引入的ender.js与JQuery发生了冲突。

>[Ender](http://ender.jit.su/) is a full featured package manager for your browser.

暂时就用纯JS写吧，只要能显示和隐藏就行。

```javascript
window.onscroll = function(){ 
    var t = document.documentElement.scrollTop || document.body.scrollTop;  
	document.getElementById("back-to-top").style.display= t>400 ? "block":"none";
};
```
是不是觉得右下角的back-to-top长得很眼熟？

###Adblock谨慎使用
一切就绪，习惯性地用F12一下，看看有没有报错，果然有两个js悲剧了，一个是百度统计，另一个是disqus的count.js。想了半天不知道是什么原因，后来看了看另外几个网站，同样是这个问题。这应该就不是代码的问题了，换了IE访问，一切正常。目光移至Chrome右上角鲜红的Adblock，就是它了！今后调试网页千万别忘记把它disable掉。

###保存主题
辛辛苦苦改了这么多，千万别忘了保存主题，因为安装新主题时sass和source中的文件都会变新主题替换掉。将sass和source中改过的文件保存在.theme文件夹下相应的旧主题中，替换掉旧的文件即可。

