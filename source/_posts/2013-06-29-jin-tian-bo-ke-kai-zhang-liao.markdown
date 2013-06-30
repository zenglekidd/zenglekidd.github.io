---
layout: post
title: "今天博客开张了"
date: 2013-06-29 21:44
comments: true
categories: 
---

开张！今天这个博客就算正式开张了

简单介绍一下，这个博客主要是个人的技术博客, 专注iOS方向, 大家一起学习一起进步！

那今天的话题呢，就从这个博客的安装开始吧：

安装Octopress
------
安装Octopress最好的方法当然是去[Octopress Setup主页](http://octopress.org/docs/setup/)了

如果不想看也不要紧，下面我把他们摘录下来：

{% codeblock 下载安装Octopress的方法. %}
git clone git://github.com/imathis/octopress.git octopress
cd octopress 
ruby --version  # Should report Ruby 1.9.3
gem install bundler
bundle install
rake install
{% endcodeblock %}

在这里稍微解释下：

1. 前三行拷贝git repo，检查ruby版本号是否正确，如果有问题 请用rvm切换到1.9.3
2. 第四行是确认你安装了bundler
3. 第五行是用bundler来安装其他依赖的gems
4. 第六行是自定义的rake行为

那我对最后一行(第六行)的内容比较好奇，于是我打开了octopress/Rakefile, 来看看rake install的行为.
下面是截取的install部分:

{% codeblock install section of Rakefile lang:ruby %}
desc "Initial setup for Octopress: copies the default theme into the path of Jekyll's generator. Rake install defaults to rake install[classic] to install a different theme run rake install[some_theme_name]"
task :install, :theme do |t, args|
  if File.directory?(source_dir) || File.directory?("sass")
    abort("rake aborted!") if ask("A theme is already installed, proceeding will overwrite existing files. Are you sure?", ['y', 'n']) == 'n'
  end
  # copy theme into working Jekyll directories
  theme = args.theme || 'classic'
  puts "## Copying "+theme+" theme into ./#{source_dir} and ./sass"
  mkdir_p source_dir
  cp_r "#{themes_dir}/#{theme}/source/.", source_dir
  mkdir_p "sass"
  cp_r "#{themes_dir}/#{theme}/sass/.", "sass"
  mkdir_p "#{source_dir}/#{posts_dir}"
  mkdir_p public_dir
end
{% endcodeblock %}

从描述中它告诉我们 这个install命令的作用呢就是安装默认的主题.

安装博客主题
------
好， 那既然说到安装主题 我们来看看有哪些不错的themes呢？

[这里](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)就是第三方的主题了
可以点击Preview预览一下 看看自己喜欢哪个

那我目前选的是fabric，(其实Greyshade给我的感觉也很好的!)

好那以fabric为例 看看怎么安装，当然这里提供[fabric主页(https://github.com/panks/fabric)
下面是我的摘录：

{% codeblock install fabric theme for Octopress %}
$ cd octopress
$ git clone git://github.com/panks/fabric.git .themes/fabric
$ rake install['fabric']
$ rake generate
{% endcodeblock %}

发布到Github Pages上面
-----
在Octopress上面也有[详细步骤](http://octopress.org/docs/deploying/github/)

当然，如果你以前没用用过Github的Pages的话 你可能也会像我一样掉到这个坑里！！
看起来很简单的setup，却在github怎么都设置不对, 我们先看看一切顺利的情况是什么样子

(未完待续。。。)

总结:以上是如何工作的
-----
