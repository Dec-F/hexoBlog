---
title: 策略模式简介
date: 2017-04-17T14:33:13.000Z
categories:
  - JavaScript
tags:
  - JavaScript
  - 设计模式
---

> 策略模式指的是定义一系列的算法，把它们一个个封装起来。将不变的部分和变化的部分隔开是每个设计模式的主题，策 略模式也不例外，策略模式的目的就是将算法的使用与算法的实现分离开来。

> 一个基于策略模式的程序至少由两部分组成。

> 1. 第一个部分是一组策略类，策略类封装了具体 的算法，并负责具体的计算过程。 
>
> 2. 第二个部分是环境类Context，Context接受客户的请求，随后 把请求委托给某一个策略类。要做到这点，说明Context中要维持对某个策略对象的引用。

<!--  more  -->
```js

// 策略组
 let strategies={
               S(base){
                    return base*4
               },
               A(base){
                   return base*3
               },
               B(base){
                   return base*2
               }
            }
            
           //Context 
            function calculate (base,lev) {
                
                   console.log(strategies[lev](base))
                
            }
            
            
            calculate(3000,'S')

```
这是策略模式的一种比较标准的实现

在JavaScript中，函数是一等对象，可以作为策略类的封装四处传递。而Context就是一个高阶函数。

策略模式在Javascript的实现非常简单方便。重写一下上面的例子。

```js
    const S = (base) =>  base*4;
    const A = (base) =>  base*3;
    const B = (base) =>  base*2;
        
    const calculate = (fn,base) => fn(base)

    console.log(calculate(S,4000))

```

用了ES6改写以后，是不是优雅了很多。


