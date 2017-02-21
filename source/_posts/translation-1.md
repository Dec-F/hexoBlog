---
title: '现代Javascript纵览--外文搬运系列（一）'
date: 2017-02-21 11:45:50
categories:
- Translation
tags: 
- JavaScript
- translation
---

**本文译自[原文链接](https://auth0.com/blog/glossary-of-modern-javascript-concepts/)**

**考虑到句子通顺，以及本人语言水平有限，有些地方没有按照原文直译。容易引起误解的地方都贴上原文了。**


<!-- more -->

## 概念
### 纯函数（Pure Functions）
一个纯函数的返回值仅由它决定的输入值，无副作用（参数）。当给定的相同的参数，结果将始终是相同的。下面是一个例子：
```js

    function add10(a){
        return a+10
    }
```

该add(a)函数接受一个数字a并返回与10相加的值。

如果我们传递的参数8给这个函数，该函数总是返回18。

对于一个确定的参数，纯函数会返回一个确定的值，不依赖和改变外部状态。

纯函数不能从父范围内引用变量，除非他们明确地传递给函数作为参数。即使这样，该函数可以不修改外部状态。
```js
    var num=10
    
    function add10(){
        return num+10
    }
    //这就不是一个纯函数
```

**总结一下**

- 纯函数必须有参数
- 同样的输入有相同输出
- 纯函数不依赖或者改变外部状态（注意console.log()改变全局状态）
- 纯函数不产生副作用
- 纯函数不能调用非纯函数

### 非纯函数（Impure Functions）
非纯函数会改变自身作用域以外的状态。一个函数运行时会改变外部状态（副作用），那么它就是非纯函数，没有返回值的函数也是非纯的。

考虑下面的例子

```js
// impure function producing a side effect
//非纯函数产生了副作用
function showAlert() {
  alert('This is a side effect!');
}

// impure function mutating external state
// 非纯函数改变了外部变量
var globalVal = 1;
function incrementGlobalVal(x) {
  globalVal += x;
}

// impure function calling pure functions procedurally
// 调用纯函数的非纯函数（调用了console.log()才是重点，所以不太理解作者要表达什么）
function proceduralFn() {
  const result1 = pureFnFirst(1);
  const result2 = pureFnLast(2);
  console.log(`Done with ${result1} and ${result2}!`);
}

// impure function that resembles a pure function,
// but returns different results given the same inputs
//看起来像是一个纯函数，但是这个函数对同一输入会返回不同输出，所以是不纯函数
function getRandomRange(min, max) {
  return Math.random() * (max - min) + min;
}
```

### 副作用
当一个功能或表达修改其自己的上下文之外的状态，其结果是一个副作用。

副作用的例子包括制作一个API的调用，操纵DOM，抛出一个警告对话框，写入数据库等。如果功能产生副作用，它被认为是不纯的。引起副作用的函数是难以预料和难以测试，因为它们会导致以外的局部范围内变化。

## 状态和无状态
状态指的是程序访问，并且可以在一个时间点上操作的信息。这包括存储在存储器中，以及OS的存储器，输入/输出端口，数据库等的数据。例如，变量在任何给定时刻的应用程序的内容是代表应用程序的的状态。
### 有状态（Stateful）
有状态的程序，应用程序或组件的数据存储在有关当前状态的内存。他们可以修改状态以及访问它的历史。下面的例子是有状态：
```js
// stateful 
//有状态
var number = 1;
function increment() {
  return number++;
}
increment(); // global variable modified: number = 2
            //全局变量被更改
```
### 无状态（Stateless）
无状态的函数和组件每一次执行都像是第一次执行。

这意味着他们不引用或不在其执行前引用任何信息，无状态是引用透明。

函数只依赖自身的参数而且不需要也不访问自身作用域外的任何东西。

请看下面例子：
```js
// stateless
//无状态
var number = 1;
function increment(n) {
  return n + 1;
}
increment(number); // global variable NOT modified: returns 2
                    //全局变量未改变
```
无状态应用程序仍然管理状态，然而他们是返回当前状态，而不不改变以前的状态（However, they return their current state without mutating previous state.怎么翻译都觉得别扭，贴上原文），这是函数式编程的一个宗旨。

## 不变性和可变性（Immutability and Mutability）
JavaScript的不可变性和可变性的概念比其他一些编程语言更加模糊不清，然而当你阅读JS中的函数式编程时会听到很多不可变性的概念。知道这些概念是如何使用和在JavaScript中实现是非常重要的。简单的定义：

### 不可变性（Immutable）
如果一个对象是不可变的。则它的值不能在创建以后修改。

### 可变性（Mutable）
如果一个对象是可变的。则它的值能在创建以后修改。

### Javascript中的可变性和不可变性
