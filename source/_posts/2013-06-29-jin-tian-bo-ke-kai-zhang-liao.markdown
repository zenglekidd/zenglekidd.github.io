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
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress 
$ ruby --version  # Should report Ruby 1.9.3
$ gem install bundler
$ bundle install
$ rake install
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

好那以fabric为例 看看怎么安装，当然这里提供[fabric主页](https://github.com/panks/fabric)
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

首先关于Github Pages的介绍 在[这里](https://help.github.com/articles/user-organization-and-project-pages)

有三种Pages: User Pages, Organization Pages and Project Pages

下面是在Github上面很重要的一句：

> Tip: You can only use your own account name for a User or Org Pages repository. A repository like joe/bob.github.io will not build Pages.

这个就是告诉我们，如果要创建User Page的话 joe/bob.github.io这样就不行， 必须使用joe/joe.github.io, 我就是被这个坑了很久.
所以第一件事情是创建一个Github repo，并且这个repo的名字一定要是joe.github.io, 比如我这个blog名字就必须是zenglekidd.github.io,

好，那创建好Github repo之后呢， 我们需要连接这个repo和我们的octopress文件夹，如何连接呢？ Octopress提供了一个方便的方法：
使用Octopress提供的rake工具

``` 
$ rake setup_github_pages
```

用这个命令就会弹出一个提示问你要链接哪个URL，填入git@github.com:zenglekidd/zenglekidd.github.io.git, 这样就完成的绑定工作

下面每次修改完Blog之后 你要做下面几件事情：

1. git push origin source, 把整个Blog的源代码push到github上面 作为backup
2. rake generate, 生成需要的html文件(把markdown转换成html)
3. rake deploy, 把生成的html文件发布到你的github主页上面

总结:以上是如何工作的
-----
那本篇博客的全部内容 特别是上面这三步 刚开始可能会弄不懂什么意思， 我来解释一下：

首先你可以在octopress/ 文件夹下面执行两个命令： 1. git branch, 2. git remote -v.
这里是我执行的结果

(未完待续)
