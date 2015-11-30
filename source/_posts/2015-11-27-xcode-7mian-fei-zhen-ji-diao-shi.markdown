---
layout: post
title: "Xcode 7免费真机调试"
date: 2015-11-27 14:33:55 +0800
comments: true
categories: 
---

现在Xcode 7可以免费进行真机调试了，不需要任何开发者账号，只要有一个Apple ID就可以了。

主要步骤如下：
1 运行Xcode后，点击菜单中的Preferences, 进行Account标签，选择添加Apple Id
2 输入AppId，登录；登录成功后会看到相关信息；
3 选中账号信息，点击View Details, 在弹出的对话框中操作添加开发者证书
4 打开要调试的项目，在Target的General的Team中可以看到刚才添加的Apple Id
5 连上设备调试前可以看到Team旁有一个Issue, 点击Fix，profile就生成并且添加到项目中了
6 可以到原来设置profile和证书的地方看看，一切都设置好了。然后就可以运行调试并且安装到真机上了。

全程傻瓜式操作；晚点上图：）

不过会不会有什么坑暂时还不知道，等遇到了再更新；至少现在是用得好好的，不需要开发者账号也可以愉快的真机调试.
