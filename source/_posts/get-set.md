---
title: getter,setter简单数据响应
date: 2017-04-12 13:44:10
categories:
- JavaScript
tags: 


---

## 知识点

1. get,set语法

> The get syntax binds an object property to a function that will be called when that property is looked up.
>
> get 语法将一个对象属性绑定到查询该属性时将被调用的一个函数上


> The set syntax binds an object property to a function to be called when there is an attempt to set that property.
>
> set 语法将一个对象属性绑定到尝试设置该属性时将被调用的一个函数上

2. Object.defineProperty()方法
<!--more-->

> The Object.defineProperty() method defines a new property directly on an object, or modifies an existing property on an object, and returns the object.
>
>Object.defineProperty（）方法直接在对象上定义一个新属性，或修改对象上的现有属性，并返回该对象。

更具体的细节，请到MDN查询。

其中我们需要用到的就是Object.defineProperty()方法可以绑定或者修改属性的get和set
```js
        let obj = {
            name: 'FXH',
            age: '12'
        }
        function bindGetSet(obj) {
            let val;
            for (let key in obj) {
                //这里可以加上hasOwnProperty过滤
                //如果是多层引用，可以使用递归
                val = obj[key];
                (function (key, val) {         //闭包分别保存键值对
                    Object.defineProperty(obj, key, {
                        enumerable: true,
                        configurable: true,
                        get: function () {
                            console.log('访问了' + key);
                            return val
                        },
                        set: function (newVal) {
                            console.log('设置了' + key)

                            console.log('新值为' + newVal)
                            return val = newVal;
                        }
                    })
                })(key, val)
            }
        }
        bindGetSet(obj);
```
控制台输出为
```js
>obj.age
 访问了age
 "12"
>obj.name
 访问了name
 "FXH"
>obj.age=21
 设置了age
 新值为21
 21
 
```

## 实现一个简单的观察者

ES5的特性让我们实现观察者变的非常简单

下面的代码就是把上面的例子补全了功能，比较清晰简单，直接上代码。

```js
function Observer(data){
            this.data=data;
            this.walk(data)
        }
        Observer.prototype.walk=function (obj){
            let val;
            for(let key in obj){
                if(obj.hasOwnProperty(key)){
                    val=obj[key];
                    if(typeof val==='object'){
                        new Observer(val);
                    }

                    this.convert(key,val)
                }
            }
        }
        Observer.prototype.convert=function(key,val){
            Object.defineProperty(this.data,key,{
                enumerable:true,
                configurable:true,
                get:function(){
                    console.log(`你访问了${key}`);
                    return val
                },
                set:function(newVal){
                    console.log(`你设置了${key}`);
                    console.log(`新${key}值为${newVal}`);
                    val=newVal;
                    return val
                }
            })
        };
        let data={
            name:'FXH',
            age:'26',
            tags:{
                hobby:'lol',
                home:'hangzhou'
            }
        }
        let a=new Observer(data)
        
```
