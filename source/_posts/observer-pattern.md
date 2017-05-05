---
title: 观察者模式（发布-订阅）简介
date: 2017-04-18 13:01:38
categories:
  - JavaScript
tags:
 - 设计模式
 - JavaScript
---


> 发布—订阅模式又叫观察者模式，它定义对象间的一种一对多的依赖关系，当一个对象的状 态发生改变时，所有依赖于它的对象都将得到通知。在JavaScript开发中，我们一般用事件模型 来替代传统的发布—订阅模式。

作为Javascript开发者，对事件模型肯定不会陌生，原生Javascript自不必说。无论是jQuery还是vue、React都有基于原生事件的API。我们先来看原生的事件模型


<!--more -->

```js


let inputNode = document.getElementById('test');

//观察者
const listener = function() {
    if(this.value){
    console.log(this.value);
    
    //移除观察者
    inputNode.removeEventListener('blur',listener)
    }
}

//注册观察者
inputNode.addEventListener('blur',listener);

inputNode.value='233';


//创建事件对象
let eve= document.createEvent('MouseEvents');
eve.initEvent('blur')


//触发事件
inputNode.dispatchEvent(eve);

```

这就是事件模型的简单演示，完整的还包括事件捕获，事件冒泡等等。

我们精简一下事件的写法，大概如下

```js

    let node={} 
    
    node.on('test',fn1,fn2)
    
    node.off('test',fn1)
    
    node.emit('test')
```

下面我们来实现这样一个模式

```js
//事件构造函数
        function Event () {
            this.events={}
        }
 
 //添加事件，可一次传多个handle
        Event.prototype.on=function(eventType,...cb) {
            if(!this.events[eventType]){
                this.events[eventType]=[]
            }
            this.events[eventType].push(...cb)
        };
//移除事件
        Event.prototype.off=function(eventType) {
            if(this.events.hasOwnProperty(eventType)){
                this.events[eventType]=null
            }
        }
//触发事件
        Event.prototype.emit= function(eventType,...args) {
            if(this.events[eventType]){
                this.events[eventType].forEach(function(cb) {
                    cb(...args)
                });
            }
        }  

```