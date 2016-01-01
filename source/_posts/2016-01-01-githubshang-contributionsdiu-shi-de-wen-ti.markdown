---
layout: post
title: "github上Contributions丢失的问题"
date: 2016-01-01 14:09:54 +0800
comments: true
categories: 工具 git
---

前几天无意中瞟了一眼自己的github, 虽然自己都怎么向github上提交代码，但是contributions明显不对，中间空了好几个月，再仔细看看，发现提交上去的作者都链接不到自己的profile, google一下答案就找到了，因为自己机器重做后忘记设置git 的user email了，而github是通过user email来对应到实际的用户的；解决这个问题一般通过命令

	$ git config --global user.email "xxxx@xxxx.com"

其中的email地址需要和自己github profile的email地址一样;

而如果希望不在commit中暴露自己的email地址的话，可以用如下命令

	$ git config --global user.email "username@users.noreply.github.com"
	
其中`username`是自己的github用户明名，这样既可以同自己的profile关联起来，自己在profile中将email地址不对外开放的话，别人也看到你的email地址。

但是`git config --global`这个命令是全局生效的，也就是所有git的提交都会使用这个email, 如果自己还使用了另外一个email提交代码，比如说一般公司内网都会搭建gitbucket之类，自己同时还在内网提交代码，那么所用的账户对应的email可能是不一样的，那么就需要对具体某个local repo来设置email了，命令如下：

	$ git config --local user.email "username@users.noreply.github.com"

这样就可以对当前的repo设置一个email, 在当前repo下所有commit都会和这个email关联起来，其实这样也挺麻烦的，按道理说应该可以配置得更加智能些，毕竟这些config信息都是存在配置文件上的，改天自己实践过后再来更新这篇文章吧！话说我发现我的某个分支好像又是存在问题的，commit的作者显示不正确，看来改天又要去检查下啦。。。。

参考资料:

* [Why are my contributions not showing up on my profile?](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/)
* [Keeping your email address private](https://help.github.com/articles/keeping-your-email-address-private/)