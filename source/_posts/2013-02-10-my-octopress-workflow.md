title: "my octopress workflow"
comments: true
categories: 学习思考
tags: 
  - Octopress
---

以前用wordpress的时候，总是觉得写一篇博客挺麻烦，要登录后台在线写文章，还要提防着浏览器突然崩溃导致辛辛苦苦码的字全部付之东流的危险。当然也可以先用在本地写好文章再copy上去，但又觉得这种方式实在是缺乏美感，写博客的欲望也大大降低。换成Octopress以来，前面的问题似乎都解决了。不过，如果能进一步减少码字之前的准备工作，一键完成`rake new_post[]`，并用喜欢的编辑器 [Sublime Text 2](http://www.sublimetext.com) 直接开始编辑，那就更好了。于是自己把git shell里的命令集写成一个批处理，很简单的几行，这样就不用每次打开shell手动切换目录并rake了。

下面是操作流程，网速比较慢，所以只传了个分辨率低的。
<!--more-->
<div class="video-container">
  <embed src="http://player.youku.com/player.php/sid/XNTEzNzcwNDI0/v.swf" allowFullScreen="true" quality="high"  align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed> 
</div>


```sh new.bat
@echo off
cd /d E:/Github/blog
rake new_post[]
```

那怎样自动使用sublime打开新创建的post文件呢？也很简单，找到octopress根目录下的`Rakefile`文件，找到新建post的task，在末尾加上一句代码即可：

```ruby Rakefile
...
...

# usage rake new_post[my-new-post] or rake new_post['my new post'] or rake new_post (defaults to "new-post")
desc "Begin a new post in #{source_dir}/#{posts_dir}"
task :new_post, :title do |t, args|
  if args.title
    title = args.title
  else
    title = get_stdin("Enter a title for your post: ")
  end
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(source_dir)
  mkdir_p "#{source_dir}/#{posts_dir}"
  filename = "#{source_dir}/#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
    post.puts "comments: true"
    post.puts "categories: "
    post.puts "---"
  end
   system "start \"D:\Sublime Text 2\sublime_text.exe\" #{filename}"		#调用sublime打开新建的post，别忘了路径中含有空格要用`“”`
end

...
...
```
修改完Rakefile后，双击运行`new.bat`，按照提示输入文章title，sublime就能自动打开新创建的文件，这时就捋起袖子开始写文章吧！当然如果你写完文章后也不想每次都打开shell进行`rake　generate` 或 `rake preview` 甚至是 `rake deploy`,都可以用批处理一键实现。别忘了给bat文件在桌面创建快捷方式，再配上生动的图标~

(p.s. 推荐一个下载图标的网站 [http://sc.chinaz.com/tubiao/](http://sc.chinaz.com/tubiao/))