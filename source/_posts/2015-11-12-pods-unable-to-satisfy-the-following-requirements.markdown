---
layout: post
title: "Pods: Unable to satisfy the following requirements"
date: 2015-11-12 18:06:36 +0800
comments: true
categories: 工具, 经验教训
---

自己在AFNetworking基础上封装了个网络库给组里用，依赖于AFNetworking, podspec里依赖版本没有写死，如下

	s.dependency "AFNetworking", "~> 2.5"
	
再加上我们为了避免每个人都pods update一下，所以都是把pods文件夹上传到了代码服务器，然后就看到小伙伴们的提交中经常带有pods文件夹下AFNetworking的改动，原因就是大家的第三方库的pods并不总是最新的，(一般在更新pods文件夹的时候，避免更新repo可以加快速度,基本上都会加上参数--no-repo-update), 所以AF的版本从2.6.0到2.6.2都有。

讨论后，索性就是写死固定的版本号啦；看了下，2.6.0肯定是要的，因为修正了很多Xcode 7里面会报warning的问题；2.6.2在2.6.0基础上差别不大，所以就写固定所依赖库的版本号：

	s.dependency "AFNetworking", "2.6.2"
	
完了在自己的Demo上运行的时候就会报错：**"Pods: Unable to satisfy the following requirements"**

其实原因很简单，因为AFNetworking 2.6.0以上都不支持iOS 6.0了，而我的Demo工程里面Podfile中版本号仍然是6.0, 考虑到现在绝大部分应用都不需要支持6.0了，索性就将Demo和podspec的版本号全部改成iOS 7.0就解决问题啦。

话说，也有人在AFNetworking上报issue提到这个问题：^_^

https://github.com/CocoaPods/CocoaPods/issues/4373