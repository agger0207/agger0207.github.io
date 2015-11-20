---
layout: post
title: "每周文章选读 第二周"
date: 2015-11-20 18:35:26 +0800
comments: true
categories: 
---


## 一 [Swift中的设计模式](http://www.infoq.com/cn/articles/design-patterns-in-swift?utm_campaign=rightbar_v2&utm_source=infoq&utm_medium=articles_link&utm_content=link_text)

InfoQ上的文章，主要介绍了在Swift中涉及到的一些设计模式，尤其是一些在语言层面就可以很好的支持的设计模式，同时也可以通过这篇文章加深对Swift语言和设计模式本身的理解。例如，作者提到，Swift支持函数式编程范式，那么高阶函数的使用就可以直接替换掉原有的命令模式(Command模式)。同理，Swift中的一些新语言特性也能够帮我们更好的理解策略模式，抽象工厂模式，适配器模式等。

所以语言上对设计模式有原生的支持后，实际的编码过程就会更加容易。

## 二 [整理iOS9适配中出现的坑](http://www.cnblogs.com/dsxniubility/p/4821184.html)

从github上的个人介绍来看，作者似乎是美团的，这篇文章主要就是记录了实际开发过程中适配iOS 9遇到的问题，主要包括：iOS 9默认使用HTTPS； Bitcode等等，很有实用价值，省得大家走弯路了。

## 三 [iOS 保持界面流畅的技巧](http://blog.ibireme.com/2015/11/12/smooth_user_interfaces_for_ios/#more-41893)

作者详细的分析了iOS界面构建中的各种性能问题以及对应的解决思路，尤其是对于列表界面。界面的优化很多时候都是用空间换时间，多做缓存，预先加载，提前进行异步绘制，避免离屏渲染等等都能有效的提升界面的流畅度，另外，我个人真心觉得要多用Instruments里面的Time Profiler、Core Animation、GPU Driver等来检测界面性能，找出真正的性能瓶颈然后有针对性的优化。

[AsyncDisplayKit](https://github.com/facebook/AsyncDisplayKit)我之前没有用过，这篇文章有提到，有空可以研究一下。

## 四 [AsyncDisplayKit技术分析](http://xujim.github.io/ios/2014/12/07/AsyncDisplayKit_inside.html)

上面刚刚提到了AsyncDisplayKit, 我自己就又找到一篇介绍的技术文章，有空可以结合源代码看看:)

## 五 [iOS图片加载速度极限优化—FastImageCache解析](http://blog.cnbang.net/tech/2578/)


又是一篇关于性能优化的文章，主要是对于[FastImageCache](https://github.com/path/FastImageCache)的分析，介绍FastImageCache是如何对含有大量图片的列表进行性能优化的。FastImageCache的滑动确实很流畅，我有个很旧的iPhone 4, 滑动的时候FPS还能有差不多50左右。

## 六 [DKNightVersion 的实现 --- 如何为 iOS 应用添加夜间模式](http://draveness.me/dknightversion-de-shi-xian-wei-ios-ying-yong-tian-jia-ye-jian-mo-shi/)

主要介绍通过runtime来支持夜间模式，这个机制应该也可以用来支持通用的换肤机制。

七 [浅析MagicalRecord](http://ddrccw.github.io/2014/05/19/a-brief-analysis-and-tips-on-magialrecord/)

网易同事写的，还没来得及仔细看，主要是对[MagicalRecord](https://github.com/magicalpanda/MagicalRecord)的一些介绍.



