---
layout: post
title: "那些我问过的、答过的、见过的iOS面试题（一）"
date: 2015-11-27 14:33:18 +0800
comments: true
categories: 
---

网上现在流传不少iOS面试题以及解答，再加上自己也整理过一些面试题给组里的人去做校招和社招的面试，正好列在下面，逐步完善和补充；有些还是我被人面试的时候回答过的，由简单到难慢慢列着吧.

## 一 Objective-C属性关键字

常见问题：
1 有哪些属性关键字，各自的作用是什么？
2 strong和weak的区别
3 weak，assign的区别
4 strong/retain和copy的区别
5 什么情况下需要用copy?
6 IBOutlet属性里面用weak, 如果用strong会有问题吗？如果有，会有什么问题？
7 默认的属性关键字，尤其是针对内存管理上的默认属性关键字
8 readonly有什么作用？
9 使用atomic会有什么负面影响? atomic修饰属性后，一定是线程安全的吗？如果atomic修饰的是NSMutableArray呢？
10 delegate为什么要用weak来修饰？如果用assign修饰的话会有问题吗？如何避免这一问题？
11 深拷贝与浅拷贝相关

一般挑几个大致问下就差不多了，遇到过好些不知道weak和assign的区别....
话说我一般不问getter和setter。。。。

## 二 基本知识
1 protocol是什么？可以定义property吗？如果可以，那么怎么做或者有什么需要注意的地方？
2 category是什么？主要有什么作用？在什么情况下会考虑用到category?category可以替换掉原有的实现吗？写category需要注意些什么（比如命名上）？Category可以定义property吗？如果可以，那么怎么做？Category可以为类增加成员变量吗？


## 三 内存管理
1 内存管理机制
2 循环引用的理解，遇到过哪些循环引用，如何解决
3 AutoReleasePool的原理，放入AutoReleasePool中的对象会在什么时候真正释放，什么情况下需要手写AutoReleasePool块
4 MRC转到ARC后需要注意的一些问题，在哪些情况下需要自己去做一些资源释放的问题
5 CF对象的Release问题相关

## 四 不分类了

发现好难分类，不分类了，一顺写下来吧！

1 View Controller的生命周期; loadView会在哪个过程中被调用到？viewWillLayoutSubviews和viewDidLayoutSubviews又会在哪个过程中被调用到？从A页面调到B页面，各自的ViewController中哪些方法会被调用到？
如果是iOS 6以前就开始做了的，那么会问下UIViewDidUnload相关的问题，比如为什么现在UIViewDidUnload不需要处理了.
2 Application的生命周期；一般需要做哪些处理；在各个阶段的回调处理时需要注意什么，比如，是否可以在启动的回调中做一些耗时操作等等;
3 APNS基本流程；应用需要做哪些处理
4 SetNeedsDisplay是做什么用的？drawRect呢？setNeedsLayout呢？

待续