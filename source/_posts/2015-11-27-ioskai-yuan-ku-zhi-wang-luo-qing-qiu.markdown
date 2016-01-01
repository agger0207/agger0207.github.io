---
layout: post
title: "iOS开源库 之 网络请求"
date: 2015-11-27 14:31:27 +0800
comments: true
categories: iOS 开源库
---

iOS一般都是HTTP/HTTPS网络请求，相应的有多个开源库来支持iOS客户端HTTP请求; 比较知名和常用的主要有如下几个：


## 一 ASIHttpRequest
比较老的开源网络库，现在已经不再维护了；ASIHttpRequest基于CFNetworking而不是NSURLConnection, 所以性能会更好；功能上会比AFNetworking更强大，但是使用起来不如AFNetworking那么方便，而且对于新的iOS版本没有提供支持；现在应该还有一些比较老的项目仍然在使用。

项目地址：https://github.com/pokeb/asi-http-request

ASI VS AFNetworking是之前大家经常讨论的话题，而且确实在支持的功能上也有一些不同，比如ASI支持同步请求，支持自定义Proxy设置(AFNetworking在iOS 7.0之前不支持),不过随着ASI不再维护，慢慢的也就没有这个比较了，可以参看下面的文章了解二者的差异.

[对比iOS网络组件：AFNetworking VS ASIHTTPRequest](http://www.infoq.com/cn/articles/afn_vs_asi)

## 二 AFNetworking

github地址：https://github.com/AFNetworking/AFNetworking

现在知名度最高、应用最广泛的开源网络库，使用起来非常简单；接口清晰明了；此外，还有很多在此基础上做的扩展，后面提到的RestKit和YTKNetworking其实下面都是在用AFNetworking做请求的发送；事实上，现存的绝大多数APP中，如果仅仅是支持HTTP协议的话，除了自己用苹果的API(NSURLSession或者NSURLConnection)外，基本就是用AFNetworking了.

AFNetworking现在到了3.0版本，3.0版本去除了NSURLConnection那套接口，只有NSURLSession的接口，只支持iOS 7以上；
另外，作者还开发了Swift版本[Alamofire](https://github.com/Alamofire/Alamofire). 可以查看RayWenderlich提供的教程[alamofire-tutorial](http://www.raywenderlich.com/85080/beginning-alamofire-tutorial)或者该教程的翻译版本[Alamofire网络库基础教程：使用 Alamofire 轻松实现 Swift 网络请求](http://www.cocoachina.com/ios/20141202/10390.html)获得更多信息.

AFNetworking本身也是很值得学习的库，里面对于NSOperation的使用、网络请求相关的处理都很值得学习，这里有一篇关于AFNetworking源码解读的文章:[AFNetworking2.0源码解析](http://blog.cnbang.net/tech/2320/)，不过并没有涉及到NSURLSession部分。
另外，由于AFNetworking的作者Mattt Thompson本身是[NSHipter](http://nshipster.com)的发起人，所以也可以看作者自己写的介绍文章[AFNetworking 2.0](http://nshipster.cn/afnetworking-2/)，体会架构的优化与了解功能的变更. 另附[NSHipter中文版本](http://nshipster.cn/).

各个大版本的变迁：
[AFNetworking 3.0 Migration Guide](https://github.com/AFNetworking/AFNetworking/wiki/AFNetworking-3.0-Migration-Guide)      

[AFNetworking 2.0 Migration Guide](https://github.com/AFNetworking/AFNetworking/wiki/AFNetworking-2.0-Migration-Guide)

## 三 RestKit
项目地址：https://github.com/RestKit/RestKit

RestKit is a modern Objective-C framework for implementing RESTful web services clients on iOS and Mac OS X. It provides a powerful object mapping engine that seamlessly integrates with Core Data and a simple set of networking primitives for mapping HTTP requests and responses built on top of AFNetworking.

RestKit主要是在网络请求的发送之外额外提供了对象映射的功能，可以将服务器返回的结果(无论是JSON还是XML)自动映射成为Model对象，并且集成了CoreData的支持；它利用RequestDescriptor和ResponseDescriptor来描述请求与Model的对应关系，这样对于配置好后的RKObjectManager, 可以自动的对请求的结果进行映射；所以其强大之处就在于这个对象的映射功能，避免繁杂的JSON映射啊什么的.

不过好像RestKit在国内的应用不多，我个人觉得是有这么几个原因：

1. RestKit底下其实还是用AFNetworking来发送请求，而且还使用的是老的AFNetworking 1.x的版本；由于其与AFNetworking耦合比较紧，替换成为AFNetworking 2.x以上版本非常麻烦；RestKit的开发者本来计划是要升级到AFNetworking 2.x的，不过弄了一两年也没搞完，现在的方向貌似是直接抛弃AFNetworking，直接自己通过NSURLSession来发送网络请求，但是具体啥时候能够做完就不知道了；
2. RestKit的设计、实现和使用都偏复杂，很多地方都会看到对这一点的抱怨；对于中小型应用，AFNetworking + Mantle 完全可以做掉 RestKit的工作，还更简单；想单独的替换/升级Mantle或者AFNetworking都方便很多；
3. 国内好像CoreData用得应该不太多，RestKit强大的ObjectMapping+CoreData的集成没有多少用武之地.
4. 中小型项目用RestKit嫌麻烦，大型项目要用RestKit倒是好，但是一般这种级别的项目或者公司都会希望能够集成自己的私有TCP协议，这个要集成起来也够头疼.

我个人也不是太喜欢RestKit, 还是AFNetworking + Mantle会更简单，即使要支持Object Mapping, 在一些JSON<>Model转换库的基础上做做也还好，毕竟，大多数时候，不需要考虑一些奇怪的JSON返回内容，可以约束服务器开发人员返回更友好更易快速解析的JSON内容，而不是一些不规范的、异构的内容.

## 四 MKNetworkKit
Github地址：https://github.com/MugunthKumar/MKNetworkKit
介绍文章：http://www.cnblogs.com/scorpiozj/p/3222803.html
License:MIT License

主要特: 

* 超轻量级
* 整个应用共享单一队列
* 正确的显示网络连接的标志
* Auto queue sizing 自动队列大小
* Auto caching 自动缓存
* Operation freezing 操作冻结
* Image Caching 图片缓存
* Performance 性能
* Full support for Objective-C ARC 全面支持Ojbective-C ARC

个人感觉:

1. 功能偏简单；库本身的设计上感觉没有什么特别的；主要就是封装了一个Operation和一个调度的Engine; Engine中涉及一些配置和调度；
2. 使用的时候需要自己去写MKNetworkEngine的子类; 当然，一个应用一般是访问同一个服务器，所以只需要在同一个Engine类中开放不同的API接口；每个接口里面去创建一个MKNetworkOperation;
3. 一般来说，不需要子类化MKNetworkOperation类
4. 创建MKNetworkOperation时参数要自己组装；所以实际使用的时候，还是需要在这之上封装一个网络层出来；
5. 坦白说，MKNetworkKit的代码风格都比较差，包括空行和空格什么的，可读性不那么好，没有其他几个知名库那样赏心悦目.


## 五 YTKNetwork 

YTKNetwork 是猿题库 iOS 研发团队基于 AFNetworking 封装的 iOS 网络库，其实现了一套 High Level 的 API，提供了更高层次的网络访问抽象。
项目地址: https://github.com/yuantiku/YTKNetwork

总体还是基于AFNetworking 2.x的，做了一些上层封装，集成了Cache功能，让网络层请求写起来更加简单。我觉得大多数情况下，使用这个库足够了，方便又清晰.

不过，我觉得实际使用的过程中，要是能够加上JSON-Model的映射功能，使用起来会更爽；另外，这套请求还是基于AFNetworking 2.x老的那套接口，也就是AFURLSessionManager那一套，底下还是NSURLConnection, 估计后面也会迁移到AFNetworking 3.x上使用NSURLSession吧！


另外，安居客的作者貌似也开源过一套网络库的上层封装，大致思想和YTKNetwork差不太多，我没有细看，据说用起来也很Happy.

全文完。

