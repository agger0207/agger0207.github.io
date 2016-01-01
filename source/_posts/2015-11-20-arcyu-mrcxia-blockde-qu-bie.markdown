---
layout: post
title: "ARC与MRC下 block的区别"
date: 2015-11-20 19:34:23 +0800
comments: true
categories: iOS
---

主要区别：

1. 在ARC开启的情况下，将只会有 NSConcreteGlobalBlock 和 NSConcreteMallocBlock 类型的 block； 而在MRC下，会有NSConcreteGlobalBlock, NSConcreteStackBlock, NSConcreteMallocBlock三种；而NSConcreteStackBlock这种block是保存在栈中的，需要注意在函数返回时会被销毁。
2. 在ARC下`__block id x`是会retain 对象x的，而MRC下这样写不会retain x, 所以MRC下__block关键字可以用于打破block使用时的循环引用，而ARC下还是要用__weak, 原文如下：

`In manual reference counting mode, __block id x; has the effect of not retaining x. In ARC mode, __block id x; defaults to retaining x (just like all other values). To get the manual reference counting mode behavior under ARC, you could use __unsafe_unretained __block id x;. As the name __unsafe_unretained implies, however, having a non-retained variable is dangerous (because it can dangle) and is therefore discouraged. Two better options are to either use __weak (if you don’t need to support iOS 4 or OS X v10.6), or set the __block value to nil to break the retain cycle.`

出自[Transitioning to ARC Release Notes](https://developer.apple.com/library/mac/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html). 
 


其余详细的对于block的剖析可以参见唐巧的文章: 

[谈Objective-C Block的实现](http://blog.devtang.com/blog/2013/07/28/a-look-inside-blocks/)
