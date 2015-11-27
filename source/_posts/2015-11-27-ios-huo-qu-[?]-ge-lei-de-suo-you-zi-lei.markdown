---
layout: post
title: "iOS 获取一个类的所有子类"
date: 2015-11-27 14:23:20 +0800
comments: true
categories: 
---

代码如下：

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
	
	//+ (NSArray *)subclassesOfClass:(Class)baseClass {
	//    int numClasses;
	//    Class *classes = NULL;
	//    numClasses = objc_getClassList(NULL, 0);
	//    
	//    NSMutableArray *classNameList = [NSMutableArray array];
	//    if (numClasses > 0) {
	//        classes = (__unsafe_unretained Class *)malloc(sizeof(Class) * numClasses);
	//        numClasses = objc_getClassList(classes, numClasses);
	//        for (int i = 0; i < numClasses; i++) {
	//            Class iterClass = classes[i];
	//            // TODO: 有些系统的类例如__NSMessageBuilder调用isSubclassOfClass时会Crash.
	//            if ([iterClass isSubclassOfClass:baseClass]) {
	//                NSString *className = NSStringFromClass(classes[i]);
	//                NSLog(@"%@", className);
	//                if ([className length] > 0) {
	//                    [classNameList addObject:className];
	//                }
	//            }
	//        }
	//        
	//        free(classes);
	//    }
	//    
	//    return classNameList;
	//}
	
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