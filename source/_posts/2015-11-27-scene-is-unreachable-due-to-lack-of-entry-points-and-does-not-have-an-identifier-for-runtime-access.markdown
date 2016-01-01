---
layout: post
title: "Scene is unreachable due to lack of entry points and does not have an identifier for runtime access"
date: 2015-11-27 14:24:38 +0800
comments: true
categories: iOS 经验技巧
---

如果在使用Storyboard中出现如下警告：

	warning: Unsupported Configuration: Scene is unreachable due to lack of entry points and does not have an identifier for runtime access via -instantiateViewControllerWithIdentifier:
	
那么说明为了访问该Scene需要给其设置一个StoryboardID,一般设置一个Storyboard ID即可.

如下图:
![img](/images/StoryboardID.jpg)
	