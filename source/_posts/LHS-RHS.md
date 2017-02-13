---
title: 'LHS/RHS查询——向下一层系列（一）'
date: 2017-02-13 16:15:07
categories:
- JavaScript
tags: 
- 向下一层
- JavaScript
---
> 这是向下一层系列第一篇，向下旨在敦促自己向底层靠近，多问why。
> 
> 由于经验还很少，不对的地方，感谢指正

本人非科班出身，所以刚接触编程的时候会有很多常识性问题，比如
```js

var imageArr=[],newData=[];

imageArr=imageArr+newData;

```
现在看着很正常，但当时想为什么左侧的`imageArr`是赋值，右面是取值，出现在一个表达式是怎么运算的呢。

这里就是JavaScript中的LHS和RHS查询的区别。
<!-- more -->

**简单来说，赋值运算的左侧会执行LHS查询，右侧则执行RHS查询。**

- LHS是寻找一个容器（地址），用来存放数值。
- RHS是寻找一个源值，即 “取得某某的值” 。

更重要的是，在很多地方，赋值运算并不是显式的，例如形参

```js
function test(par){
    alert(par)
}

test(22)

```
最后调用`test()`并传参时，会先进行一次LHS查询，声明一个变量为par，并把22赋值给par；
`alert(par)`时执行一次RHS查询。

其实在调用`test()`时也是一次RHS查询
