---
layout: post
title: "iOS 获取一个类的所有子类"
date: 2015-11-27 14:23:20 +0800
comments: true
categories: iOS
---

利用Objetive-C runtime提供的方法objc_getClassList可以获取到当前源码中所有注册的类的信息，该方法的官方文档如下:

```Objective-C
/** 
 * Obtains the list of registered class definitions.
 * 
 * @param buffer An array of \c Class values. On output, each \c Class value points to
 *  one class definition, up to either \e bufferCount or the total number of registered classes,
 *  whichever is less. You can pass \c NULL to obtain the total number of registered class
 *  definitions without actually retrieving any class definitions.
 * @param bufferCount An integer value. Pass the number of pointers for which you have allocated space
 *  in \e buffer. On return, this function fills in only this number of elements. If this number is less
 *  than the number of registered classes, this function returns an arbitrary subset of the registered classes.
 * 
 * @return An integer value indicating the total number of registered classes.
 * 
 * @note The Objective-C runtime library automatically registers all the classes defined in your source code.
 *  You can create class definitions at runtime and register them with the \c objc_addClass function.
 * 
 * @warning You cannot assume that class objects you get from this function are classes that inherit from \c NSObject,
 *  so you cannot safely call any methods on such classes without detecting that the method is implemented first.
 */
OBJC_EXPORT int objc_getClassList(Class *buffer, int bufferCount)
    __OSX_AVAILABLE_STARTING(__MAC_10_0, __IPHONE_2_0);
```

获取一个类的直接子类的主要思路就是，先获取到类的个数，然后分配足够的空间获取类的信息，接下来遍历所有的类并且比较基类从而判断是否该类的直接子类，代码如下所示：

```Objective-C
+ (NSArray *)directSubclassesOfClass:(Class)baseClass {
   int numClasses;
   Class *classes = NULL;
   numClasses = objc_getClassList(NULL, 0);
   
   NSMutableArray *classNameList = [NSMutableArray array];
   if (numClasses > 0) {
       classes = (__unsafe_unretained Class *)malloc(sizeof(Class) * numClasses);
       numClasses = objc_getClassList(classes, numClasses);
       for (int i = 0; i < numClasses; i++) {
           if (class_getSuperclass(classes[i]) == baseClass){
               NSString *className = NSStringFromClass(classes[i]);
               NSLog(@"%@", className);
               if ([className length] > 0) {
                   [classNameList addObject:className];
               }
           }
       }
       
       free(classes);
   }
   
   return classNameList;
}
```
	
但是上面的方法是不能获取到子类的子类的，只能获取到直接子类；我尝试将判断条件
	
	if (class_getSuperclass(classes[i]) == baseClass)
	
替换成为	

	if ([iterClass isSubclassOfClass:baseClass])

结果Crash了。

原因是：

1. 官方文档中明确说明，`"You cannot assume that class objects you get from this function are classes that inherit from \c NSObject"`, 也就是获取到的类可能根本就不是NSObject的子类，而isSubclassOfClass是NSObject的类方法；
2. 在这个过程中，会执行+initialzie方法，在这个过程中部分系统类会提示找不到selector而crash；（具体原因没有去研究过，因为第一条原因就决定了不能这样用了）	
	            	
那么只能循环一下继续找基类来获取所有子类：

```Objective-C
+ (NSArray *)subclassesOfClass:(Class)baseClass {
    int numClasses;
    Class *classes = NULL;
    numClasses = objc_getClassList(NULL, 0);
    
    NSMutableArray *classNameList = [NSMutableArray array];
    if (numClasses > 0) {
        classes = (__unsafe_unretained Class *)malloc(sizeof(Class) * numClasses);
        numClasses = objc_getClassList(classes, numClasses);
        for (int i = 0; i < numClasses; i++) {
            Class iterClass = classes[i];
            if (nil == iterClass) {
                continue;
            }
            
            Class superClass = class_getSuperclass(iterClass);
            while (nil != superClass) {
                if (superClass == baseClass){
                    [classNameList addObject:iterClass];
                    break;
                }
                
                superClass = class_getSuperclass(superClass);
            }
        }
        
        free(classes);
    }
    
    return classNameList;
}
```
	
	
当然，一般情况不太会有这种需求，即使有这种需求的时候，都是需要找某个自己写的特殊类的子类，也一般有别的方法来解决，但是用来写写测试代码还是很方便的，比如我自己封装了一个网络请求的库，所有的请求都从基类的request派生都有同样的调用方式，于是可以用这种方法来快速测试某个项目中所有的请求在默认设置下是否会出错。

另外，获取所有类的这个方法在第三方日志库CocoaLumberjack中也有用到，下面是其中一段源码：

```Objective-C
+ (NSArray *)registeredClasses {

    // We're going to get the list of all registered classes.
    // The Objective-C runtime library automatically registers all the classes defined in your source code.
    //
    // To do this we use the following method (documented in the Objective-C Runtime Reference):
    //
    // int objc_getClassList(Class *buffer, int bufferLen)
    //
    // We can pass (NULL, 0) to obtain the total number of
    // registered class definitions without actually retrieving any class definitions.
    // This allows us to allocate the minimum amount of memory needed for the application.

    NSUInteger numClasses = 0;
    Class *classes = NULL;

    while (numClasses == 0) {

        numClasses = (NSUInteger)MAX(objc_getClassList(NULL, 0), 0);

        // numClasses now tells us how many classes we have (but it might change)
        // So we can allocate our buffer, and get pointers to all the class definitions.

        NSUInteger bufferSize = numClasses;

        classes = numClasses ? (Class *)malloc(sizeof(Class) * bufferSize) : NULL;
        if (classes == NULL) {
            return nil; //no memory or classes?
        }

        numClasses = (NSUInteger)MAX(objc_getClassList(classes, (int)bufferSize),0);

        if (numClasses > bufferSize || numClasses == 0) {
            //apparently more classes added between calls (or a problem); try again
            free(classes);
            numClasses = 0;
        }
    }

    // We can now loop through the classes, and test each one to see if it is a DDLogging class.

    NSMutableArray *result = [NSMutableArray arrayWithCapacity:numClasses];

    for (NSUInteger i = 0; i < numClasses; i++) {
        Class class = classes[i];

        if ([self isRegisteredClass:class]) {
            [result addObject:class];
        }
    }

    free(classes);

    return result;
}
```

全文完.	