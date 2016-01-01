---
layout: post
title: "如何创建自己的私有Pod Spec Repo"
date: 2016-01-01 14:10:19 +0800
comments: true
categories: 工具
---

晕，发现文章在公司写了没传到github上；假期结束后去公司发好了；其实很简单，直接看官网文章[Private Pods](https://guides.cocoapods.org/making/private-cocoapods.html)就可以了，另外有一篇中文文章写得更细致点。

提一句，CocoaPods Spec Repo的核心也就是一个github仓库[CocoaPods/Specs](https://github.com/CocoaPods/Specs), 然后这里面的所有内容就是各个开源库提交过来的Specs信息，`pod setup`的时候会将这个仓库clone到本地；为什么我们经常说在国内，`pod update`的时候非常慢，主要就是因为需要将本地的spec repo和github上做一个同步，而这个repo由于里面的文件非常多，更新也多，所以以国内的网速，同步起来非常非常慢，我试过将这个仓库一次性全clone到本地，要一个多小时。。。。所以一般大家在`pod update`的时候会加上参数`--no-repo-update`避免重新去同步这个仓库。

而所谓的创建私有Pod Spec Repo或者说创建一个新的Repo镜像，本质都是维护一个github的仓库，这个仓库里面放的就是这些spec了

话说创建好自己的私有Pod Spec Repo后，添加原有的那些内部用的podspec的时候，无一例外都报错，囧。。。。看来得花时间好好review这些podspec啦 ^_^