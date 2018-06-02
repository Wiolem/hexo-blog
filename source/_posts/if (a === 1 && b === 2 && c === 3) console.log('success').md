---
title: if (a === 1 && b === 2 && c === 3) console.log('success')
date: 2018-3-3 21:14:46
categories: Front End
tags:
- defineProperty
- vue
description: 
---
一道简单的面试题
```
if (a === 1 && b === 2 && c === 3) console.log('success')
```
```
Uncaught ReferenceError: a is not defined
```
很明显无法执行， 如何添加代码使其成立并且输出 success
既然来面试 vue , 很容易我们想到 defineProperty 属性拦截实现，代码如下：
```
var temp
Object.defineProperty(window, "a", {
  get : function(){
    console.log('a = ' + (temp + 1))
    return ++temp
  },
  set : function(newValue){
    temp = newValue
  },
  enumerable : true,
  configurable : true
})
a = 0
if(a === 1 && a === 2 && a === 3) console.log('success')
```
log
```
a = 1
a = 2
a = 3
success
```
> MDN [Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 

>静态方法 Object.defineProperty() 直接在对象上定义新属性，或者修改对象上的现有属性，并返回该对象。


如果我们降低难度怎么实现 

```
if (a == 1 && b == 2 && c == 3) console.log('success')
else console.log('error')
```
双等肯定要用隐式类型转换了

下面链接提供了一些方法，感兴趣可以看看

参考链接：
[从 (a==1&&a==2&&a==3) 成立中看javascript的隐式类型转换](https://yq.aliyun.com/articles/399499)
