---
layout: post
title: "nvm安装node后bash仍然提示找不到的问题"
date: 2015-11-20 19:57:05 +0800
comments: true
categories: 经验教训
---

一个月前，重做了系统后需要安装node.js来跑起我的测试服务器程序，那天用homebrew死活装不上，总是提示下载出错，后来查了下就用nvm安装，结果安装后后运行node提示找不到这个命令；用`nvm which node`命令查了下，貌似通过nvm安装的node在路径`/Users/username/.nvm/versions/node/v4.2.1/bin/node`下，而不是在/usr/local/bin目录下，所以提示找不到。

后来通过下面的命令解决了这个问题：

	ln -s /Users/username/.nvm/versions/node/v4.2.1/bin/node /usr/local/bin/node

实际就是创建了一个到`/usr/local/bin/node`的软连接	。

用homebrew直接安装的话没这个问题。
