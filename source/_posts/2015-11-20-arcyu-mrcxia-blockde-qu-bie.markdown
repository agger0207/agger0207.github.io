---
layout: post
title: "ARC与MRC下 block的区别"
date: 2015-11-20 19:34:23 +0800
comments: true
categories: iOS
---

核心其实就是在ARC 开启的情况下，将只会有 NSConcreteGlobalBlock 和 NSConcreteMallocBlock 类型的 block； 而在MRC下，会有NSConcreteGlobalBlock, NSConcreteStackBlock, NSConcreteMallocBlock三种；而NSConcreteStackBlock这种block是保存在栈中的，需要注意在函数返回时会被销毁。

详细可以参见唐巧的文章

[谈Objective-C Block的实现](http://blog.devtang.com/blog/2013/07/28/a-look-inside-blocks/)
