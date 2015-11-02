---
layout: post
title: "My First Post for Testing Octopress"
date: 2015-10-31 15:46:15 +0800
comments: true
categories: 
---

It is a test file

这是我使用Octopress搭建的blog的测试文章，用来看看效果如何。

主要参考教程是[Octopress 教程目录](http://shengmingzhiqing.com/blog/octopress-tutorials-toc.html/), 另外blog的一些配置参考了[南峰子的技术博客](http://southpeak.github.io/), 这也是我非常推荐的一个iOS相关的技术博客，后续我会逐步完善自己的blog内容，例如样式排版什么的.

随便贴个代码试试看:

	+ (NSArray *)getPropertyList:(Class)theClass {
	    NSMutableDictionary *dic = [NSMutableDictionary dictionary];
	    
	    unsigned int outCount, i;
	    objc_property_t *properties = class_copyPropertyList(theClass, &outCount);
	    for (i = 0; i < outCount; i++) {
	        
	        objc_property_t property = properties[i];
	        NSString *propertyNameString = [[NSString alloc] initWithCString:property_getName(property) encoding:NSUTF8StringEncoding];
	        NSString *propertyAttributeString = [[NSString alloc] initWithCString:property_getAttributes(property) encoding:NSUTF8StringEncoding];
	        
	        NSMutableString *filedTypeString = [NSMutableString stringWithString:propertyAttributeString];
	        
	        NSRange range = [filedTypeString rangeOfString:@","];
	        filedTypeString = (NSMutableString *)[filedTypeString substringToIndex:range.location];
	        
	        [dic setObject:filedTypeString forKey:propertyNameString];
	    }
	    
	    return [dic allKeys];
	}
	
参考资料：

[Octopress 教程目录](http://shengmingzhiqing.com/blog/octopress-tutorials-toc.html/)
[南峰子的技术博客](http://southpeak.github.io/)	