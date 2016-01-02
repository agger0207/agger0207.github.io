---
layout: post
title: "工作中遇到的小问题与解决方案备忘 一"
date: 2016-01-02 14:28:12 +0800
comments: true
categories: 经验技巧
---

## Xcode使用

### 单元测试覆盖率

### 多个Xcode版本混装问题

在同一台机器上安装过不同版本的Xcode的时候，在执行某些操作的时候(比如说执行git命令的时候)会提示Xcode找不到之类的错误，此时可以通过如下命令指定使用某个对应的Xcode:

	$ sudo xcode-select --switch /Applications/Xcode.app

其中，`/Applications/Xcode.app`是要使用的Xcode版本的目录，一般默认的目录就是这个.

### 检查Xcode是否正版可用

在Xcode Ghost事件后，Apple提供了命令行来检查Xcode是否正版可用.

参考链接：
