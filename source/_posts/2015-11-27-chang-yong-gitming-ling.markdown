---
layout: post
title: "常用Git命令"
date: 2015-11-27 14:26:30 +0800
comments: true
categories: 
---

## 基本操作

git clone 

git checkout

git push

git pull

git commit

git add

git branch

## 分支相关操作

## 问题处理

1 切换到另一分支例如运行命令 **git checkout develop** 时提示：

	error: The following untracked working tree files would be overwritten by checkout:
	...
	Please move or remove them before you can switch branches.
	Aborting

错误原因很清楚了，有untracked的文件未处理，如果这些文件是需要提交的，那么git commit后再切换分支;如果这些改动可能本来就不要的；那么直接执行下面命令先清理掉即可：

	git clean -d -f
	
-d参数的含义是删除untracked目录与文件;-f含义是force, 强制执行。

然后再执行 **git checkout develop** 就没问题了.	
