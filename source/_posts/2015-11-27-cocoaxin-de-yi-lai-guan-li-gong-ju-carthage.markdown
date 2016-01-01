---
layout: post
title: "Cocoa新的依赖管理工具 Carthage"
date: 2015-11-27 14:27:51 +0800
comments: true
categories: 工具
---

之前只知道使用git submodule和CocoaPods来做包的依赖管理，前段时间看一个开源库的时候，不记得是Mantle还是MagicRecord的时候发现ReadMe里面有介绍可以用Carthage来做包的依赖管理。

不过根据ReadMe中说明，只支持动态框架，所以只能用于iOS 8以及以后.

	Note that Carthage only supports dynamic frameworks, which are only available on iOS 8 or later (or any version of OS X).

github地址：

[https://github.com/Carthage/Carthage](https://github.com/Carthage/Carthage)

由于自己主要还是在用Cocoapods和git submodule，所以还是推荐两篇文章自己去看吧，也算是给自己的一个备忘:

1. [Carthage：Xcode项目的GitHub依赖管理器](http://www.infoq.com/cn/news/2015/05/carthage-dependency-manager)
2. [Cocoa 新的依赖管理工具：Carthage](http://www.isaced.com/post-265.html)






