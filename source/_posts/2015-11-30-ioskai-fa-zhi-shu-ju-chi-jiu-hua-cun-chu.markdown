---
layout: post
title: "iOS开发 之 数据持久化存储"
date: 2015-11-30 15:17:31 +0800
comments: true
categories: 
---------------------------------------

如果只是想设置默认值而不覆盖用户设置的值，那么可以通过`registerDefaults`来做.

NSUserDefaults *standardDefaults = [NSUserDefaults standardUserDefaults];
[standardDefaults registerDefaults:@{@"favoriteColor": @"Green"}];
[standardDefaults synchronize];
每次程序启动的时候调用registerDefaults: 方法都是安全的。完全可以将这个方法的调用放到applicationDidFinishLaunching:方法中. 这个方法永远都不会覆盖用户设置的值。

NSUserDefaults是线程安全的，苹果官方文档有明确说明[The NSUserDefaults class is thread-safe.](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/index.html)

[McZonk关于is NSUserDefault thread-safe的回答](http://stackoverflow.com/questions/10864482/is-nsuserdefault-thread-safe)有提到，

	Speaking for 10.10 and iOS8 if you looking into the implementation you'll find that -[NSUserDefaults setObject:forKey:] is calling __CFPreferencesSetAppValueWithContainer, which will eventually end up in +[CFPrefsSource withSourceForIdentifier:user:byHost:container:perform:]. This method is using a pthread_mutex_t to lock the access to the dictionary containing the values.

所以内部访问的时候是有加锁保护，所以是线程安全的.