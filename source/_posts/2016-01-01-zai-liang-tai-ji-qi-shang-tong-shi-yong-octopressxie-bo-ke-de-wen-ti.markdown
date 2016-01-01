---
layout: post
title: "在两台机器上同时用octopress写博客的问题"
date: 2016-01-01 13:37:15 +0800
comments: true
categories: 工具 经验技巧 git
---

问题描述:
之前是在晚上在公司机器上来发布文章，但是同时也会在自己家里写文章，我们知道，octopress日常操作的实际就是blog所在的git仓库的source分支，例如我自己的blog对应的仓库实际上就是`https://github.com/agger0207/agger0207.github.io`, 平时写好文章后提交到git上，保持同步就好了。

今天在家里执行`rake deploy`的时候就发现报错了；错误信息与解决过程看下面.

错误1:

	## Deploying branch to Github Pages 
	## Pulling any updates from Github Pages 
	cd _deploy
	rake aborted!
	Errno::ENOENT: No such file or directory @ dir_chdir - _deploy

提示是找不到`_deploy`文件夹，看了下，还真没有，感情自己一直没在家里的机器上deploy过，搜索了下，如果有不同的本地仓库的时候就会存在这种问题，执行如下命令，就会生成_deploy文件夹啦!

	$ rake setup_github_pages
		

错误2：

继续`rake deploy`, 继续报错：

	On branch master
	nothing to commit, working directory clean
	
	## Pushing generated _deploy website
	To https://github.com/agger0207/agger0207.github.io
	 ! [rejected]        master -> master (non-fast-forward)
	error: failed to push some refs to 'https://github.com/agger0207/agger0207.github.io'
	hint: Updates were rejected because the tip of your current branch is behind
	hint: its remote counterpart. Integrate the remote changes (e.g.
	hint: 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.

错误提示是说无法向自己博客所在远程仓库的`master`分支push, 因为当前本地的版本没有更新到最新版本. 了解git的同学都知道，要push到远程分支，首先需要将最新的改动pull下来，否则不让提交的。而octopress实际发布的过程实际上就是会将本地的改动push到blog所在git仓库的master分支(也就是我们日常通过编写markdown文档的过程其实并不是直接去改动blog的呈现，而是在改动后，通过`rake generate`生成网页，然后deploy的时候将生成的页面信息push到远程的master分支上).

google了一下，原来`_deploy`文件夹下的内容就是从`master`分支下取下来的内容，大家可以看下，`_deploy`目录下的是不是正好和github上`master`分支一样呢？

执行命令:
	
	$ cd _deploy
	$ ls -a 

可以看到结果：

	.		assets		favicon.png	javascripts	sitemap.xml
	..		atom.xml	images		posts		stylesheets
	.git		blog		index.html	robots.txt	
.git的存在证明了这就是一个本地的git仓库嘛；

剩下的就好办了，只需要与master同步即可：

	$ git pull origin master	
	
pull失败，提示有冲突，不要紧，执行下面的命令恢复到和master分支一样就可以了，反正这里面的内容都是自动生成的.

	$ git reset --hard origin/master

然后搞定，回到上一层目录，执行`rake deploy`就OK.

小结：
解决下来，发现这个问题其实还蛮有意思的；原来`_deploy`文件夹就是对应的git repo的master branch。其实octopress的这些操作核心就是ruby和git; 如果没有猜错，之前解决`_deploy`文件夹不存在时所执行的命令就是创建一个本地的仓库并且设置对应的remote url, 不过我对ruby不太熟，要不然可以研究下里面的rakefile确认一下。而octopress对于git仓库的处理就是，编辑的内容对应的是source branch, 而`_deploy`对应master branch, 和git submodule有点类似，只不过子文件夹下对应的remote repo是同一个repo的不同branch,  而git submodule一般都是不同的submodule放在不同的仓库下。

参考资料：

* [No such file or directory - _deploy](https://github.com/imathis/octopress/issues/334)
* [Octopress pushing error to GitHub](http://stackoverflow.com/questions/19619280/octopress-pushing-error-to-github)

