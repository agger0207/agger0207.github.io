---
layout: post
title: "修改git commit历史"
date: 2016-01-01 18:27:46 +0800
comments: true
categories: 工具 git
---

例如，一个需求是，之前递交的时候本地设置的email地址都错了，那么需要把之前所有的提交的电子邮件地址改过来，也就是批量修改commit历史，而`git commit --amend`命令只能修改最近的一次提交，这个时候就可以用到git filter-branch命令了，示例如下：

	$ git filter-branch --commit-filter '
	        if [ "$GIT_AUTHOR_EMAIL" = "xxx@xxx.com" ];
	        then
	                GIT_AUTHOR_NAME="agger0207";
	                GIT_AUTHOR_EMAIL="agger0207@users.noreply.github.com";
	                git commit-tree "$@";
	        else
	                git commit-tree "$@";
	        fi' HEAD

如果之前执行过一次，会报错，说无法backup之类，原因是会再一次rewrite .git目录下的`refs/original/`, 这时加个参数`-f`强制执行即可：

	git filter-branch -f --commit-filter '
		        if [ "$GIT_AUTHOR_EMAIL" = "xxx@xxx.com" ];
		        then
		                GIT_AUTHOR_NAME="agger0207";
		                GIT_AUTHOR_EMAIL="agger0207@users.noreply.github.com";
		                git commit-tree "$@";
		        else
		                git commit-tree "$@";
		        fi' HEAD

同理，也可以删除某个作者或者email地址对应的历史commit:

	git filter-branch --commit-filter '
	        if [ "$GIT_AUTHOR_EMAIL" = "xxx@xxx.com" ];
	        then
	                skip_commit "$@";
	        else
	                git commit-tree "$@";
	        fi' HEAD

如何判断某个提交是从哪个地址或者作者提交呢? github在自己的帮助文档[check the email address used for a commit](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/)里明确提到，在commit对应的url下加上.patch就会显示递交的作者信息了，例如, 通过url`https://github.com/octocat/octocat.github.io/commit/67c0afc1da354d8571f51b6f0af8f2794117fd10.patch`就可以得到`octocat.github.io`这个rep的commit`67c0afc1da354d8571f51b6f0af8f2794117fd10`的提交信息如下:

	From 67c0afc1da354d8571f51b6f0af8f2794117fd10 Mon Sep 17 00:00:00 2001
	From: The Octocat <octocat@nowhere.com>
	Date: Sun, 27 Apr 2014 15:36:39 +0530
	Subject: [PATCH] updated index for better welcome message

我实践的时候，前面进行的都很顺利，但是提交的时候`git push`会报错，git会认为有分支的发散，但是我试着先`git pull`在`git push`的话发现后来是多了很多commit而不是直接覆盖掉原有的commit; 后来尝试直接`git push -f`强制覆盖，貌似可以解决问题。然后看参考链接中的[git-filter-branch(1) Manual Page](https://www.kernel.org/pub/software/scm/git/docs/git-filter-branch.html)也提到，除非清楚的知道要做什么，否则谨慎使用git filter-branch, 因为无法安全的push, 原文如下：

	WARNING! The rewritten history will have different object names for all the objects and will not converge with the original branch. You will not be able to easily push and distribute the rewritten branch on top of the original branch. Please do not use this command if you do not know the full implications, and avoid using it anyway, if a simple single commit would suffice to fix your problem. (See the "RECOVERING FROM UPSTREAM REBASE" section in git-rebase(1) for further information about rewriting published history.)
	
	Always verify that the rewritten version is correct: The original refs, if different from the rewritten ones, will be stored in the namespace refs/original/.

## 参考资料

* [Git 工具 - 重写历史](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2)
* [git-filter-branch(1) Manual Page](https://www.kernel.org/pub/software/scm/git/docs/git-filter-branch.html)
* [check the email address used for a commit](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/)