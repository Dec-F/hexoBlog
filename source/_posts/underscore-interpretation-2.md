---
title: 阅读underscore系列（二）--类型检查
date: 2017-06-24 13:07:35
tags:
- underscore.js
- JavaScript
categories:
- 源码阅读笔记
---

> 原发于github。[原文链接](https://github.com/strongfanfan/underscore-interpretation/issues/2)


Javascript作为一门动态语言，类型检查无论是在工具库或者业务中都是经常用到。

先来了解一下常用的几种方法
<!--more -->
### typeof

> [MDN-typeof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)

> typeof返回一个字符串。返回值如下：
![](http://omqz8y2im.bkt.clouddn.com/underscore2-1.png)

```js
typeof ''; // string 有效
typeof 1; // number 有效
typeof true; //boolean 有效
typeof undefined; //undefined 有效
typeof null; //object 无效
typeof [] ; //object 无效
typeof new Function(); // function 有效
typeof new Date(); //object 无效
typeof new RegExp(); //object 无效
```
typeof 可以对JS基础数据类型做出准确的判断,但是对引用类型和null的判断就无能为力了

### instanceof

> [MDN-instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)

> instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

```js
[] instanceof Array //true
[] instanceof Object //true
```
就如上面测试的一样，instanceof 虽然可以准确的检测出引用类型的值，但是它是基于原型链的，这就造成了所用引用类型都是Object的实例。

那么问题来了，Object怎么检测？可能需要判断每一种引用类型，全部返回`false`，才能判断它是Object类型。心累。

### constructor


![](http://omqz8y2im.bkt.clouddn.com/underscore2-2.png)

看似很美好是吧？但是`constructor`是不稳定的。因为`prototype`被重写以后，`constructor`会默认指向`Object`。如下：
```js
function Foo () {
  }

  Foo.prototype={foo:1}

  let foo=new Foo()

 console.log(foo.constructor) //function Object() {[native code] }
```

### Object.prototype.toString


先来看下打印结果

![](http://omqz8y2im.bkt.clouddn.com/underscore2-3.png)

`[object,Array]`第一个`Object`是数据类型，而包装类型机制的存在，基础类型都会显示`Object`，`null`和`undefined`比较特殊，也会返回`object`。第二项是数据的内部`[[Class]]`属性。

> 所有 typeof 返回值为 "object" 的对象（如数组）都包含一个内部属性 [[Class]]（我们可 以把它看作一个内部的分类，而非传统的面向对象意义上的类） 。这个属性无法直接访问， 一般通过 Object.prototype.toString(..) 来查看。


underscore.js中使用的就是这种方法，源码如下：

```js

//在1210行左右

//Arguments是用来兼容<IE9
_.each(['Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp', 'Error'], function (name) {
    _['is' + name] = function (obj) {
      return Object.prototype.toString.call(obj) === '[object ' + name + ']';
    };
  });
  
```


详细的了解了类型检查，借鉴一下underscore,js的方法，在业务中写类型检查再也不会战战兢兢的了



