---
layout: post
title: "运行时报错 This application is modifying the autolayout engine from a background thread"
date: 2015-11-27 14:25:41 +0800
comments: true
categories: 经验技巧
---

下了个老的Swift的Demo, 在iOS 9模拟器上运行时报错

	This application is modifying the autolayout engine from a background thread, which can lead to engine corruption and weird crashes. This will cause an exception in a future release.
	
从错误信息中可以看出，在后台线程中修改了UI. 搜了下，NSURLSession的问题，iOS 9上，NSURLSession的回调并不是在主线程中了，那个Demo在回调中直接去修改了界面，所以会报这个错。对应的Objective-C方法如下：

	- (NSURLSessionDataTask *)dataTaskWithURL:(NSURL *)url completionHandler:(void (^)(NSData * __nullable data, NSURLResponse * __nullable response, NSError * __nullable error))completionHandler;

解决方案: 

在block中需要更新UI时，利用GCD切换到主线程即可；

	// Swift	
	dispatch_async(dispatch_get_main_queue(), {
	    // code here
	})
	
从StackOverFlow上的回答来看， NSURLConnection也存在这个问题，而且仅仅在iOS 9 SDK上才出现，改天看看AFNetworking的改动确认一下：

附链接：	

[Getting a “This application is modifying the autolayout engine” error?](http://stackoverflow.com/questions/28302019/getting-a-this-application-is-modifying-the-autolayout-engine-error)	