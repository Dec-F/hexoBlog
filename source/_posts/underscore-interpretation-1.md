---
title: 阅读underscore系列(一)--对象和函数两种接口风格的实现
date: 2017-06-24 13:07:35
tags:
- underscore.js
- JavaScript
categories:
- 源码阅读笔记
---

> 原发于github[原文链接](https://github.com/strongfanfan/underscore-interpretation/issues/1)

一直断断续续看源码，奈何水平有限，效率极低，再加上懒癌作祟，看一天歇一周的节奏，一直没感觉有什么沉淀，这次来个细水长流，每天都看一点，有什么心得就来写一篇这个系列。

老实说，underscore这个库我是没用过的，所以需要先看接口。面向接口读源码可能会更容易找到切入点。
<!--more -->

今天是第一篇，就写一下underscore的接口风格。
它支持以下写法
```js
_.map([1, 2, 3], function(n){ return n * 2; });
_([1, 2, 3]).map(function(n){ return n * 2; });
```
面向对象和函数风格都可以接受。下面结合源码，来说一下实现。



### `_` 是什么


`_`其实还是个函数，且是个构造函数。`_`可以当作对象使用，因为JS中，一切皆对象。而underscore的所用方法都挂载在`_`下。

源码如下：
```js
var _ = function(obj) {
    if (obj instanceof _) return obj;
    if (!(this instanceof _)) return new _(obj);
    this._wrapped = obj;
  };
  
```
可以看到当使用函数`_()`调用时，实际上是将`_`当作构造函数调用,并返回一个含有`_wrapped`的实例。


### 函数式调用时发生了什么

这个问题，我们就单独把上面的代码跑一下和原来的对比（自己写的用 `$()` 代替，和 `_()` 区分）

![](http://omqz8y2im.bkt.clouddn.com/underscore1-2.png)
![](http://omqz8y2im.bkt.clouddn.com/underscore1-3.png)

可以看到underscore的方法是挂载在实例的原型对象上，即`_().__proto__`

而我copy过来的方法是挂载在构造函数上的，即`$().__proto__.constructor`。其实就是上面说的 `_` 这个函数上，这时调用`$().map()`肯定会报错。

报错是因为方法没有在原型链上找到，先解决掉。其实就是把挂在`$.`上的方法一一映射到`$.prototype`上。

源码中就是这样解决的，如下：

```js

 // 便于理解，对源码略微修改了一下
 // Add your own custom functions to the Underscore object.
 // _.functions(obj)方法是把obj对象上的方法映射为一个数组，包含了所有obj下的方法名
 
  _.mixin = function(obj) {
    _.each(_.functions(obj), function(name) {
      var func = _[name] = obj[name];
      _.prototype[name] = function() {
        var args = [this._wrapped];
       Array.prototype.push.apply(args, arguments);
        return  func.apply( _ , args);
      };
    });
  };

  // Add all of the Underscore functions to the wrapper object.
  _.mixin(_);
  
```

可以看到在`_.prototype`下的方法和在`_`下的方法实现是不一样的，多了一层调用。

本质上函数式调用是对象式调用的语法糖，自动将`_()._wrapped`对象传进方法作为第一个参数，并调用对应函数。




这种形式的接口优雅且实现也不难，很值得我们借鉴和使用。


----

> 第一篇`underscore.js`源码阅读心得系列。以后会不定期更新，感兴趣的同学可以点watch，一起探讨问题。
