---
layout: post
title: "iOS开发每周文章选读 之 第四周"
date: 2015-12-04 16:02:44 +0800
comments: true
categories: iOS
---

有好几篇都是美团技术团队的文章，美团好像是大量使用了ReactiveCocoa的，在这方面的实践经验和干货非常之多。

## [一 深入理解Objective-C：方法缓存](http://tech.meituan.com/DiveIntoMethodCache.html)

来自美团技术团队，文章从runtime源码的角度探究了Objective-C在runtime层的方法决议（Method resolving）过程和方法缓存（Method cache）的实现，分析了Objective-C为什么要做方法缓存，为什么要这么做方法缓存，以及方法缓存在什么地方等等，对理解Objective-C底层机制很有帮助，也可以体会这种不断优化的过程。

另外，关于方法缓存的优化中，还有一点，Objective-C在从散列表里检索方法的时候，规避掉了对方法名的字符串比较，而是直接用指针比较来做的以提高性能，而可以这样做的前提就是，所有method cache里面存放同一个方法名的都是同一个指针。

## [二 深入理解Objective-C：Category](http://tech.meituan.com/DiveIntoCategory.html)

来自美团技术团队，文章主要从runtime源码的角度来剖析category的实现原理，包括其数据结构，加载机制等等；然后其实根据其原理我们还可以找到被覆盖掉的原有类的方法，因为Category添加的方法总是在前面的。

不过有一个问题我到现在还没有想到好的解决方法，就是如何判断一个类的属性或者方法是否通过Category添加的：（

### [三  RACSignal的Subscription深入分析](http://tech.meituan.com/RACSignalSubscription.html)

来自美团技术团队，主要是对ReactiveCocoa的订阅机制进行分析，并重点讨论了subscription的副作用；其实，如果能够对RAC的源码调试一两遍会更清楚。

至于subscription的副作用，实际是因为一个cold signal在每次订阅的时候都会被触发，但是有的时候其实是只希望触发一次的，比如说一个网络请求，对这个请求的订阅可能只是要知道最后请求发送的结果，而并不希望每次都重新触发一次，这篇文章就分析了与之相关的一些操作符，例如`replay`之类。

## [四 细说ReactiveCocoa的冷信号与热信号 (一)](http://tech.meituan.com/talk-about-reactivecocoas-cold-signal-and-hot-signal-part-1.html)

美团技术团队的系列文章，主要是介绍ReactiveCocoa的冷信号与热信号的区别，同系列的两篇文章为:

* [细说ReactiveCocoa的冷信号与热信号（二）：为什么要区分冷热信号](http://tech.meituan.com/talk-about-reactivecocoas-cold-signal-and-hot-signal-part-2.html)
* [细说ReactiveCocoa的冷信号与热信号（三）：怎么处理冷信号与热信号](http://tech.meituan.com/talk-about-reactivecocoas-cold-signal-and-hot-signal-part-3.html)


## 五 [ReactiveCocoa Tutorial – the Definitive Introduction](http://southpeak.github.io/blog/2014/08/02/reactivecocoazhi-nan-%5B%3F%5D-:xin-hao/)

ReactiveCocoa入门文章的翻译，译者的blog就在我的友情链接处，原文在此处：  
http://www.raywenderlich.com/62699/reactivecocoa-tutorial-pt1

同样也是分为了两部分.

## 六 [MVVM Tutorial with ReactiveCocoa](http://southpeak.github.io/blog/2014/08/08/mvvmzhi-nan-yi-:flickrsou-suo-shi-li/)

同上，也是ReactiveCocoa+MVVM的入门文章的翻译, 原文：
http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1

吐槽一句，有部分人认为MVVM就等同于ReactiveCocoa, 这个认识明显是不对的，可以参见唐巧的那篇[被误解的 MVC 和被神化的 MVVM](http://blog.devtang.com/blog/2015/11/02/mvc-and-mvvm/), ReactiveCocoa对于MVVM的主要意义是可以方便的实现数据绑定，从而能够很方便的更新界面.

## 七 [在ReactiveCocoa中将一个ViewModel绑定到UITableView上](http://southpeak.github.io/blog/2014/09/21/zai-reactivecocoazhong-jiang-%5B%3F%5D-ge-viewmodelbang-ding-dao-uitableviewshang/)

同一个人翻译的，原文链接：  
http://blog.scottlogic.com/2014/05/11/reactivecocoa-tableview-binding.html

这篇文章主要就是介绍，ReactiveCocoa如何对UITableView进行数据的绑定，因为一般来说，使用RAC对于普通的view进行绑定是相对容易很多的，这里面介绍了原作者写的一个辅助类来做UITableView的绑定.

全文完。