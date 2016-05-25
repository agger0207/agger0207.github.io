---
layout: post
title: "iOS开发每周文章选读 之 第五周"
date: 2015-12-11 13:53:35 +0800
comments: true
categories: iOS
---



## [一 深入理解Objective-C：方法缓存]()

来自美团技术团队，文章从runtime源码的角度探究了Objective-C在runtime层的方法决议（Method resolving）过程和方法缓存（Method cache）的实现，分析了Objective-C为什么要做方法缓存，为什么要这么做方法缓存，以及方法缓存在什么地方等等，对理解Objective-C底层机制很有帮助，也可以体会这种不断优化的过程。

另外，关于方法缓存的优化中，还有一点，Objective-C在从散列表里检索方法的时候，规避掉了对方法名的字符串比较，而是直接用指针比较来做的以提高性能，而可以这样做的前提就是，所有method cache里面存放同一个方法名的都是同一个指针。