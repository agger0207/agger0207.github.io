---
layout: post
title: "YYModel源码略读"
date: 2015-11-20 20:13:42 +0800
comments: true
categories: iOS
---

现在iOS开发中总会用到各种JSON模型转换库，比如JSOMModel，Mantle, MJExtension等等；前段时间又了解到一个简单的JSON模型转换库叫做[YYModel](https://github.com/ibireme/YYModel), 作者博客上有很多好文章，我经常去读的；其中一篇就说提到了[iOS JSON 模型转换库评测](http://blog.ibireme.com/2015/10/23/ios_model_framework_benchmark/)。

JSON模型转换库的源代码里面值得去看的主要就是其中对于runtime的运用，因为基本上不用runtime就干不了这个活。而YYModel也比较简单，对于想多了解runtime应用的人来说其实是比较方便的，而且作者将runtime获取到类的信息，属性信息，方法信息都封装成了ObjC的类，这样大家如果想获取这些信息的话，就不需要自己去重新获取了。

例如其中的YYClassInfo类的定义如下：

	@interface YYClassInfo : NSObject
	
	@property (nonatomic, assign, readonly) Class cls;
	@property (nonatomic, assign, readonly) Class superCls;
	@property (nonatomic, assign, readonly) Class metaCls;
	@property (nonatomic, assign, readonly) BOOL isMeta;
	@property (nonatomic, strong, readonly) NSString *name;
	@property (nonatomic, strong, readonly) YYClassInfo *superClassInfo;
	
	@property (nonatomic, strong, readonly) NSDictionary *ivarInfos;     ///< key:NSString(ivar),     value:YYClassIvarInfo
	@property (nonatomic, strong, readonly) NSDictionary *methodInfos;   ///< key:NSString(selector), value:YYClassMethodInfo
	@property (nonatomic, strong, readonly) NSDictionary *propertyInfos; ///< key:NSString(property), value:YYClassPropertyInfo
	
	+ (instancetype)classInfoWithClass:(Class)cls;
	
	+ (instancetype)classInfoWithClassName:(NSString *)className;
	
	@end

看上去是不是直观了很多，实际上就是对runtime的那些方法做了一些上层的封装，里面用到的方法也还是**class_copyPropertyList**， **class_copyMethodList**这些。但封装之后看上去就直观了很多，对runtime不那么了解的人也可以获取到这些信息了，可以根据类名字轻易获取到类的信息，里面方法列表，属性列表等统统都有。

另，使用**class_copyPropertyList**时候需要注意，**class_copyPropertyList**只能获取到定义在自己的属性，而不能获取定义在父类的属性，官方文档有这样一段注释：
	
	Any properties declared by superclasses are not included. 

所以在_YYModelMeta的- (instancetype)initWithClass:(Class)cls方法中，可以看到下面的代码：

    // Create all property metas.
    NSMutableDictionary *allPropertyMetas = [NSMutableDictionary new];
    YYClassInfo *curClassInfo = classInfo;
    while (curClassInfo && curClassInfo.superCls != nil) { // recursive parse super class, but ignore root class (NSObject/NSProxy)
        for (YYClassPropertyInfo *propertyInfo in curClassInfo.propertyInfos.allValues) {
            if (!propertyInfo.name) continue;
            if (blacklist && [blacklist containsObject:propertyInfo.name]) continue;
            if (whitelist && ![whitelist containsObject:propertyInfo.name]) continue;
            _YYModelPropertyMeta *meta = [_YYModelPropertyMeta metaWithClassInfo:classInfo
                                                                    propertyInfo:propertyInfo
                                                                         generic:genericMapper[propertyInfo.name]];
            if (!meta || !meta->_name) continue;
            if (!meta->_getter && !meta->_setter) continue;
            if (allPropertyMetas[meta->_name]) continue;
            allPropertyMetas[meta->_name] = meta;
        }
        curClassInfo = curClassInfo.superClassInfo;
    }
    if (allPropertyMetas.count) _allPropertyMetas = allPropertyMetas.allValues.copy;
        
在这段代码中可以看到会去找到除了NSObject和NSProxy外所有父类的所有属性，最后拿到这个类定义的所有属性.

另，在YYClassPropertyInfo的- (instancetype)initWithProperty:(objc_property_t)property方法中可以看到下面的代码：

	objc_property_attribute_t *attrs = property_copyAttributeList(property, &attrCount);
    for (unsigned int i = 0; i < attrCount; i++) {
        switch (attrs[i].name[0]) {
            case 'T': { // Type encoding
                if (attrs[i].value) {
                    _typeEncoding = [NSString stringWithUTF8String:attrs[i].value];
                    type = YYEncodingGetType(attrs[i].value);
                    if (type & YYEncodingTypeObject) {
                        size_t len = strlen(attrs[i].value);
                        if (len > 3) {
                            char name[len - 2];
                            name[len - 3] = '\0';
                            memcpy(name, attrs[i].value + 2, len - 3);
                            _cls = objc_getClass(name);
                        }
                    }
                }
            } break;
            case 'V': { // Instance variable
                if (attrs[i].value) {
                    _ivarName = [NSString stringWithUTF8String:attrs[i].value];
                }
            } break;
            case 'R': {
                type |= YYEncodingTypePropertyReadonly;
            } break;
            case 'C': {
                type |= YYEncodingTypePropertyCopy;
            } break;
            case '&': {
                type |= YYEncodingTypePropertyRetain;
            } break;
            case 'N': {
                type |= YYEncodingTypePropertyNonatomic;
            } break;
            case 'D': {
                type |= YYEncodingTypePropertyDynamic;
            } break;
            case 'W': {
                type |= YYEncodingTypePropertyWeak;
            } break;
            case 'P': {
                type |= YYEncodingTypePropertyGarbage;
            } break;
            case 'G': {
                type |= YYEncodingTypePropertyCustomGetter;
                if (attrs[i].value) {
                    _getter = [NSString stringWithUTF8String:attrs[i].value];
                }
            } break;
            case 'S': {
                type |= YYEncodingTypePropertyCustomSetter;
                if (attrs[i].value) {
                    _setter = [NSString stringWithUTF8String:attrs[i].value];
                }
            } break;
            default:
                break;
        }
    }
    
这些'D'还有'R'实际上就是说明了这个property的一些属性修饰符，详细的可以参见Apple的文档    
[Declared Properties](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtPropertyIntrospection.html)

https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtPropertyIntrospection.html