---
layout: post
title: "Xcode Error: Use of undeclared type"
date: 2015-11-29 14:23:53 +0800
comments: true
categories: 经验技巧
---

前几天写swift + watchOS 的程序时，编译遇到个错误，提示"Use of undeclard type"; 但实际上是系统类型，而且也正确import了module;后来仔细看了下，发现原来是加错了target导致的；修改成为正确的Target之后问题解决：

如下图：

![img](/image/XcodeTarget.jpg)

出错的时候，所有Target是全选的。

我后来新建了个文件试下，默认所有的Target都是选中的，当时没注意，一路Next下去了；后来突然想起前几天好像装了个Xcode插件叫做**AllTargets**的，这个插件就是当添加文件到工程的时候会自动帮助选上所有的Target，后来想想，这个功能没啥用，赶紧将这个插件删除了，然后就不会有这个问题啦:)

**AllTargets**如下图所示：

![img](/image/XcodeAllTargets.jpg)

