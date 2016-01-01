---
layout: post
title: "Cocoapods和Git submodules结合使用"
date: 2015-11-20 19:41:59 +0800
comments: true
categories: 工作
---

iOS下第三方库管理一般就是CocoaPods和Git Submodule这两种方法（当然，现在可能还需要加上[Carthage](https://github.com/Carthage/Carthage)）, 这两种的基本使用方法就不说了，相关介绍的文章一大把，一般用起来也比较简单。不过前不久遇到个问题，是需要两个结合使用的。

情况是这样的，有个Library B，是部门B提供的，只以submodule的方式提供；然后我需要封装的另外一个库A, 需要依赖于B, 然后A需要以CocoaPods的方式提供给别的team使用，这两个都是公司内部的库。然后我需要在A里面描述清楚对B的依赖关系，而且希望使用者直接在PodFile里面加上A的描述就好了，大致搜了一下，找到了解决方法。

首先，实现A的过程中，A还是通过submodules的方法来使用B,具体方法就不说了，无非是git submodule add, git submodule init和git submodule update这些命令；

然后在使用A的时候，podfile里面额外添加 :submodules => true.

例如如果库A的名字叫做AggerLib的话，那么PodFile里面就是

	pod 'AggerLib', :git => 'https://github.com/git/agger/AggerLib.git', :branch => 'master', :submodules => true
	
Done.

## 参考资料  
1. [USING SUBMODULES IN COCOAPODS SOURCED FROM GIT](http://www.geero.net/2014/06/using-submodules-in-cocoapods-sourced-from-git/)			
