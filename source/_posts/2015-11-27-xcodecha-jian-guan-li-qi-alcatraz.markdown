---
layout: post
title: "Xcode插件管理器Alcatraz"
date: 2015-11-27 14:20:53 +0800
comments: true
categories: 工具
---

Xcode插件管理器Alcatraz可以帮助搜索、查看和安装各类Xcode插件，这样不需要每次再去每个插件的主页上去找安装帮助文档了，极大的简化了Xcode插件的安装与管理.

Alcatraz本身就是一个Xcode的插件，所以按照正常的插件安装方法就可以了。

直接从项目主页上拷贝过来的

Installation

To install, open up your terminal and paste this:

	curl -fsSL https://raw.github.com/alcatraz/Alcatraz/master/Scripts/install.sh | sh
	
or download the repository from Github and build it in Xcode. You'll need to restart Xcode after the installation.

Alcatraz requires Xcode Command Line Tools, which can be installed in Xcode via Preferences > Downloads.

简单翻译一下：
两种安装方法，一种方法是直接在命令行中执行那个命令就可以了；
第二种方法是从github上clone代码到本地，然后在Xcode中编译，接着重新启动Xcode就可以了，这个时候会有提示是否要load bundles，选择load就可以了。
第二种方法其实就是绝大多数插件比如VVDocumeter的手动安装方法。

安装好之后，在Xcode的Window菜单中就可以看到“Package Manager”这个菜单项，点开后就可以去里面搜索安装管理其他插件了。

Xcode菜单项:

![image](/images/PackageManager.jpg)



Alcatraz管理页面:

![image](/images/PackageManagerUI.jpg)


地址：[https://github.com/alcatraz/Alcatraz](https://github.com/alcatraz/Alcatraz)