---
layout: post
title: "使用Octopress搭建博客流程"
date: 2015-11-02 20:11:23 +0800
comments: true
categories: 工具
---

## 常见操作

以下操作都是在octopress目录下进行.

### 添加新文章
1 执行如下命令

	rake new_post["title"] 
	
可以在 source/_posts 目录下创建一篇新博文, 文章是采用[Markdown语法](http://wowubuntu.com/markdown/)进行书写的, 直接使用Markdown的免费编辑器来书写就好了；我暂时用的是Mou, 不过在编辑比较长的文章后会有卡顿，以后慢慢会尝试别人推荐的Sublime Text 3 + MarkdownEditing进行书写。

2 执行如下命令进行预览

	rake preview 

然后在浏览器中输入http://localhost:4000/ 就可以预览.
	
### 生成与发布
执行如下命令生成页面与发布页面.
	
	rake generate # 生成页面
	rake deploy	  # 发布到站点
	
从执行的时候命令行输出的信息可以看出，生成页面的命令实际上是在public目录生成html页面，而deploy命令实际上就是将public页面中的数据push到你的git仓库的master分支上，而我们日常编辑的文章原始内容就与source分支对应。

因此，为了保存本地的更改，那么可以提交自己的源文件，并且push到远程source分支上.

	git add .
	git commit
	git push origin source

同样，如果你像我一样，看到别人漂亮的排版后想参考对方的源文件，那么可以从github上找到对方对应的仓库，切换到source分支然后参考对方的源文件, 比如_config.yml是怎么写的啊，等等...

### 添加友情链接
在_config.yml文件中的default_asides栏中添加一项，然后在对应目录下添加一个html文件.

例如我自己的_config.yml文件中对应的内容为：

	default_asides: [asides/about.html, asides/recent_posts.html, asides/friendly_link.html]

那么在source->_includes->asides文件夹下要保证有这几个文件，html文件里面的内容无非就是添加一个标题和链接，比如:

	<section>
	  <h1>友情链接</h1>
	  <ul id="friendly_link">
	    <li class="post">
	      <a href="http://blog.devtang.com">唐巧的技术博客</a>
	    </li>
	  </ul>
	</section>

### 调整列表缩进
当写好文章发布后，我发现在Markdown里面添加的列表的缩进存在问题，列表符号（或者编号）默认溢出左侧文字内容边界, 没有和上下文对齐，显得非常难看，例如本文末尾的参考链接里的列表，而调整正常的方法就是在`sass/custom/_layout.scss`中找到`//$indented-lists: true;`然后再去掉注释符号`//`即可开启列表自动缩进的功能。

### 添加导航栏上的链接
在`source/_includes/custom/navigation.html`里可以设置导航栏上显示的内容和链接，例如，如果想要在导航栏上显示`首页`, `工具`, `所有文章`这几项，那么`navigation.html`的内容应该类似下面这样子:

	<ul class="main-navigation">
	  <li><a href="{{ root_url }}/">首页</a></li>
	  <li><a href="{{ root_url }}/blog/categories/gong-ju">工具</a></li>
	  <li><a href="{{ root_url }}/blog/archives">所有文章</a></li>
	</ul>

链接怎么知道呢？在`_deploy`目录下实际放着的就是这个博客对应的网页文件，在`blog/categories`目录下就是所有的分类，那么这个url其实就是和文件的相对路径对应的了。

### 添加分类
新生成的Markdown文件中，有`categories:`内容，直接在后面写上分类的名字即可；如果一篇文章属于多个分类，那么多个分类之间用空格隔开即可。

### 代码着色
直接用Markdown的语法即可为代码着色。

### 编辑格式调整
基本就是普通的Markdown编辑，后面会写一篇介绍如何使用Markdown的文档；实在不行，参照别人git上的源码整整或者google一下，方便又快捷。

## 评论系统
TODO: 使用多说的评论系统

## 如何加快访问速度
TODO

## 参考链接
如下几个著名的blog都是机遇Octopress搭建的，我也参考了他们的源码. 其中有好几个都是非常优秀的iOS工程师, 里面的文章都非常值得一读.

1. [南峰子的技术博客](http://southpeak.github.io/)   
2. [雷纯锋的技术博客](http://blog.leichunfeng.com/)

其中还有几篇讲解如何搭建的文章, 其实我上面写的基本上都来自于下面的教程，然后再加上自己的一些实际操作经验.

3. [Octopress 教程目录](http://shengmingzhiqing.com/blog/octopress-tutorials-toc.html/)  
4. [使用 Octopress+GitHub Pages 搭建个人博客](http://blog.leichunfeng.com/blog/2014/11/11/use-octopress-plus-github-pages-to-setup-a-personal-blog/)     
5. [唐巧的技术博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)