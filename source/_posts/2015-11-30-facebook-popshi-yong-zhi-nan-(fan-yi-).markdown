---
layout: post
title: "Facebook Pop使用指南（翻译）"
date: 2015-11-30 16:03:28 +0800
comments: true
categories: iOS, 开源库
---

原文链接：

[maxmyers/FacebookPop](https://github.com/maxmyers/FacebookPop)


#使用Facebook Pop库的五个步骤#

```objective-c
  // 1. 选择一种动画类型 
  // 存在如下几种动画类型: POPBasicAnimation  POPSpringAnimation POPDecayAnimation
  POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];

  // 2. 决定你是要针对View的属性做动画还是Layer的属性做动画, 这里我们选择一个View的属性kPOPViewFrame, 顾名思义，这是针对View的Frame变化做动画
  // View的属性包括： kPOPViewAlpha kPOPViewBackgroundColor kPOPViewBounds kPOPViewCenter kPOPViewFrame kPOPViewScaleXY kPOPViewSize
  // Layer的属性包括 - kPOPLayerBackgroundColor kPOPLayerBounds kPOPLayerScaleXY kPOPLayerSize kPOPLayerOpacity kPOPLayerPosition kPOPLayerPositionX kPOPLayerPositionY kPOPLayerRotation kPOPLayerBackgroundColor
  basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewFrame];

  // 3. 确定使用下面其中一种方法来设置toValue
   //  anim.toValue = @(1.0);
  //  anim.toValue =  [NSValue valueWithCGRect:CGRectMake(0, 0, 400, 400)];
  //  anim.toValue =  [NSValue valueWithCGSize:CGSizeMake(40, 40)];
  basicAnimation.toValue=[NSValue valueWithCGRect:CGRectMake(0, 0, 90, 190)];

  // 4. 为动画创建名字以及设置Delegate
   basicAnimation.name=@"AnyAnimationNameYouWant";
   basicAnimation.delegate=self

  // 5. 将动画附加到View或者Layer, 这里我们选择self.tableView
  // 如果我们之前设置的是针对Layer的属性，那么我们可以选择一个Layer例如self.tableView.layer
  [self.tableView pop_addAnimation:basicAnimation forKey:@"WhatEverNameYouWant"];
```


## 第一步 选择一种动画类型 


### POPBasicAnimation
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.timingFunction=[CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionLinear];
// kCAMediaTimingFunctionLinear  kCAMediaTimingFunctionEaseIn  kCAMediaTimingFunctionEaseOut  kCAMediaTimingFunctionEaseInEaseOut
```

### POPSpringAnimation
```objective-c
POPSpringAnimation *springAnimation = [POPSpringAnimation animation];
springAnimation.velocity=@(1000);       // change of value units per second
springAnimation.springBounciness=14;    // value between 0-20 default at 4
springAnimation.springSpeed=3;     // value between 0-20 default at 4
```

### POPDecayAnimation
```objective-c
POPDecayAnimation *decayAnimation=[POPDecayAnimation animation];
decayAnimation.velocity=@(233); //change of value units per second
decayAnimation.deceleration=.833; //range of 0 to 1
```

### Example
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
```

## Step 2 Decide if you will animate a view property or layer property

### View Properties
##### Alpha - kPOPViewAlpha
##### Color - kPOPViewBackgroundColor
##### Size - kPOPViewBounds
##### Center - kPOPViewCenter
##### Location & Size - kPOPViewFrame
##### Size - kPOPViewScaleXY
##### Size(Scale) - kPOPViewSize


### Layer Properties
##### Color - kPOPLayerBackgroundColor
##### Size - kPOPLayerBounds
##### Size - kPOPLayerScaleXY
##### Size - kPOPLayerSize
##### Opacity - kPOPLayerOpacity
##### Position - kPOPLayerPosition
##### X Position - kPOPLayerPositionX
##### Y Position - kPOPLayerPositionY
##### Rotation - kPOPLayerRotation
##### Color - kPOPLayerBackgroundColor

### Example
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
```

#### Note: Works on any Layer property or any object that inherits from UIView such as UIToolbar | UIPickerView | UIDatePicker | UIScrollView |  UITextView | UIImageView | UITableViewCell | UIStepper | UIProgressView | UIActivityIndicatorView | UISwitch | UISlider | UITextField | UISegmentedControl | UIButton | UIView | UITableView


## Step 3 Find your property below then add and set .toValue

### View Properties
##### Alpha - kPOPViewAlpha
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewAlpha];
basicAnimation.toValue= @(0); // scale from 0 to 1
```

##### Color - kPOPViewBackgroundColor
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPViewBackgroundColor];
basicAnimation.toValue= [UIColor redColor];
```

##### Size - kPOPViewBounds
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewBounds];
basicAnimation.toValue=[NSValue valueWithCGRect:CGRectMake(0, 0, 90, 190)]; //first 2 values dont matter
```

##### Center - kPOPViewCenter
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewCenter];
basicAnimation.toValue=[NSValue valueWithCGPoint:CGPointMake(200, 200)];
```

##### Location & Size - kPOPViewFrame
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewFrame];
basicAnimation.toValue=[NSValue valueWithCGRect:CGRectMake(140, 140, 140, 140)];
```

##### Size - kPOPViewScaleXY
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewScaleXY];
basicAnimation.toValue=[NSValue valueWithCGSize:CGSizeMake(3, 2)];
```

##### Size(Scale) - kPOPViewSize
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewSize];
basicAnimation.toValue=[NSValue valueWithCGSize:CGSizeMake(30, 200)];
```
### Layer Properties
##### Color - kPOPLayerBackgroundColor
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerBackgroundColor];
basicAnimation.toValue= [UIColor redColor];
```

##### Size - kPOPLayerBounds
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerBounds];
basicAnimation.toValue= [NSValue valueWithCGRect:CGRectMake(0, 0, 90, 90)]; //first 2 values dont matter
```

##### Size - kPOPLayerScaleXY
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerScaleXY];
basicAnimation.toValue= [NSValue valueWithCGSize:CGSizeMake(2, 1)];//increases width and height scales
```

##### Size - kPOPLayerSize
```objective-c
POPBasicAnimation *basicAnimation = [POPBasicAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPLayerSize];
basicAnimation.toValue= [NSValue valueWithCGSize:CGSizeMake(200, 200)];
```

##### Opacity - kPOPLayerOpacity
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerOpacity];
basicAnimation.toValue = @(0);
```

##### Position - kPOPLayerPosition
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerPosition];
basicAnimation.toValue= [NSValue valueWithCGRect:CGRectMake(130, 130, 0, 0)];//last 2 values dont matter
```

##### X Position - kPOPLayerPositionX
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerPositionX];
basicAnimation.toValue= @(240);
```

##### Y Position - kPOPLayerPositionY
```objective-c
POPSpringAnimation *anim = [POPSpringAnimation animation];
anim.property=[POPAnimatableProperty propertyWithName:kPOPLayerPositionY];
anim.toValue = @(320);
```

##### Rotation - kPOPLayerRotation
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerRotation];
basicAnimation.toValue= @(M_PI/4); //2 M_PI is an entire rotation
```
#### Note: Property Changes work for all 3 animation types - POPBasicAnimation POPSpringAnimation POPDecayAnimation
### Example
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName: kPOPLayerRotation];
basicAnimation.toValue= @(M_PI/4); //2 M_PI is an entire rotation
```
## Step 4 Create Name & Delegate For Animation
```objective-c
basicAnimation.name=@"WhatEverAnimationNameYouWant";
basicAnimation.delegate=self;
```
##### Declare Delegate Protocol `<POPAnimatorDelegate>`

### Delegate Methods
`
<POPAnimatorDelegate>
`
```objective-c
- (void)pop_animationDidStart:(POPAnimation *)anim;
```
```objective-c
- (void)pop_animationDidStop:(POPAnimation *)anim finished:(BOOL)finished;
```
```objective-c
- (void)pop_animationDidReachToValue:(POPAnimation *)anim;
```


### Example
```objective-c
POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewFrame];
basicAnimation.toValue=[NSValue valueWithCGRect:CGRectMake(0, 0, 90, 190)];
basicAnimation.name=@"WhatEverAnimationNameYouWant";
basicAnimation.delegate=self;
```

## Step 5 Add animation to View
```objective-c
 [self.tableView pop_addAnimation:basicAnimation forKey:@"WhatEverNameYouWant"];
```
### Example
```objective-c
  POPSpringAnimation *basicAnimation = [POPSpringAnimation animation];
  basicAnimation.property = [POPAnimatableProperty propertyWithName:kPOPViewFrame];
  basicAnimation.toValue=[NSValue valueWithCGRect:CGRectMake(0, 0, 90, 190)];
  basicAnimation.name=@"SomeAnimationNameYouChoose";
  basicAnimation.delegate=self;
  [self.tableView pop_addAnimation:basicAnimation forKey:@"WhatEverNameYouWant"];
```

