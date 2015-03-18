title: 正式进军Octopress
categories: 学习笔记
tags: 
	- Octopress
---

2013年才进入Octopress的菜鸟表示很惭愧，临近毕业，在学校一堆事情让自己也难以静下心来折腾些什么。在好好珍惜最后一个寒假的同时，也终于有空闲来完成自己早在一年多前就想做的事。

2010年刚开始使用wordpress的时候，写博客的动力还是相当充足的。那时wordpress友好的后台管理界面和日臻完善的Manual Book让我保持着不断尝试新花样的好奇心。那种自己能够自由掌控前端布局、后端行为的方式确实很棒。

可是渐渐地，每每打开wp-admin，看着形似word的编辑器，各种样式、颜色、段落等等工具挤在一起，显得繁琐而复杂，写作的欲望越来越低。直到后来，开始尝试使用Markdown写博客。之前就听说过，但并未真正去使用，以至于第一次用Markdown时就被它的轻盈、简洁所吸引。所以，Wordpress是时候该抛弃了，今天终于完成了这一重大变革！

<!--more-->

## 搭建过程

关于git的使用这里就不说了，从安装octopress开始

### 安装

``` sh Install octopress
	git clone http://github.com/imathis/octopress octopress

	cd octopress
	ruby --version # Should report Ruby 1.9.2
	gem install bundler
	bundle install 

    rake install  #install default theme
```


- Problems:	

 	You have already activated rake 10.0.3, but your Gemfile >requires rake 0.9.6. Using bundle exec may solve this.
 	这样的错误，原因是安装了rake 10.0.3，而Gemfile.lock里的需求信息是 rake 0.9.6
 	解决方法：在Gemfile里添加 ` gem 'rake','10.0.3' ` 后运行 `bundle update rake` 进行更新


### 配置

将根目录下的_config.yml的编码改成UTF-8（可以用Notepad++），修改_config.yml，包括url、title、subtitle、author等等。

- Problems:

	这里Jekyll有个小bug，在windows上会遇到编码问题。网上普遍的做法是
到Ruby的安装目次\lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\找到convertible.rb这个文件，将Line 29中
`self.content = File.read（File.join（base， name））` 改为
`self.content = File.read（File.join（base， name）， :encoding => "utf-8"）`

### 写文章
Octopress的文章以Markdown格式保存在 source\_posts 中（注意文件名格式），写新文章可以用：

```sh
rake new_post["title"]
```

### 生成页面并预览

```sh	
rake generate	#  Generates posts and pages into the public directory

rake preview	# Watches, and mounts a webserver at http://localhost:4000
```

- problems：	
	如果source文件夹中有些文件包含中文，比如_posts里的文章内容，或者自定义页面时加入了中文（“recent posts” 改为 “最新文章”等），此时会报诸如： `Liquid Exception: invalid byte sequence in UTF-8 in post` 之类的错误。花了一个多小时在网上寻找解决方法，尝试了3、4种，最后只有一种有效：在执行 `rake generate` 之前先设置控制台使之支持UTF-8格式，即运行 `chcp 65001`，再在标题栏右键->属性->字体->选择“Lucida Console”即可。

### 部署
第一次部署时要在Github上创建一个username.github.com的repo，username要和github的username一致。

```sh
rake setup_github_pages  #在Octopress目录下配置octopress与github的连接

git@github.com:username/username.github.com.git  #按照提示填入地址

rake deploy		# 将_deploy文件夹清空，将public中的内容复制到_deploy并部署到github
```

### 迁移文章和评论

1. wordpress控制台有个导出功能，将导出的xml放到`source/_post`中
2. 下载[mb.rb](https://github.com/odinyu/odinyu.github.com/blob/source/md.rb)
3. 运行：

```sh 导出wordpress文章 http://hopes4.me/blog/migrate-from-wordpress-to-octopress/ 引用
cd octopress/source/_post
ruby md.rb wordpress.xml

#如果提示少包，则依次安装如：
gem install hpricot --platform=mswin32	#这个要加特殊参数
gem install ya2yaml
```

