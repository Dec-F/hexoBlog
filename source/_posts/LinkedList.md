---
title: 'Javascript链表结构的实践'
date: 2017-02-20 14:50:12
categories:
- Javascript
tags: 
- JavaScript
- 数据结构和算法
---


首先创建一个链表
```js
 function LinkedList() {
            let Node = function (ele) {
                this.ele = ele
                this.next = null
            },
                length = 0, head = null;
            this.append = function (ele) {
                var node = new Node(ele), //{1}         
                    current; //{2} 
                if (head === null) { //列表中第一个节点 //{3}         
                    head = node;
                } else {
                    current = head; //{4} 
                    //循环列表，直到找到最后一项         
                    while (current.next) {
                        current = current.next;
                    }
                    //找到最后一项，将其next赋为node，建立链接         
                    current.next = node; //{5}     
                }
                length++; //更新列表的长度 //{6} 
            };
           this.print = function () {
            var current = head, //{1}         
                string = '';    //{2} 

            while (current) {   //{3}         
                string += current.ele; //{4}         
                current = current.next;   //{5}     
            }
            return string;                //{6} 
        };
    }
    let LinkedList1= new LinkedList();
    [1,2,3,4,5,6].forEach(function (value) {
        LinkedList1.append(value)
    })
    console.log(LinkedList1.print()); //123456
```
<!-- more -->

**问题是链表每一个项存在哪个函数的作用域？如果是append这个函数的作用域，print查找的时候非父级作用域的变量可以读取吗？**

我是这样理解的

**head只保存了链表头，就是第一个项的引用，然后读取当前项的next引用，寻找下一个项，问题是项是在append中创建的，也没有返回，print为什么可以读取append中的项？**


然后我打断点

```js
Closure (LinkedList)
Node:(ele)
head:Node
        ele:1
        next:Node
                ele:2
                next:Node
                        ele:3
                        next:Node
                                ele:4
                                next:Node   
                                        ele:5
                                        next:Node
                                                ele:6
                                                next:null
                                                __proto__:Object
                                        __proto__:Object
                                __proto__:Object
                        __proto__:Object
                __proto__:Object
        __proto__:Object
length:5


```

可以看出，所有项的引用都是在head变量中的

我们在append的else中的最后一句执行了current.next = node;，这就相当于
```js
var a = {name:'PPP'};
function b(){
  a.age = 23;
}
b()//{name:'PPP',age:23}
```

类似于这样，并没有返回a，但是a添加了属性同理，我们修改head中一层一层的next指向，因为head是外层作用域的私有变量，所以可以在print中直接获取到

head是一个头，他的内部保存的引用，或者引用的引用都是head的一部分，这就是一个多层引用，head中保留了所有项的引用

不知道理解的还有没有偏差，想到了再更新

